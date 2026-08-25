# Output

<!-- TODO: This page is currently based on ApiSurfaceRevised. Revisit it once the implementation is available and verify fields, routes, batching behavior, correction rules, and examples against the current code. -->

Records quantities produced during a production Run.

Output includes good production, waste, downgraded product, and specific reject types.

## The Output object

```json
{
  "run": "L01-260810-002",
  "kind": "good",
  "quantity": 1200,
  "unit": "pcs",
  "at": "2026-08-10T23:00:00+02:00"
}
```

Rejected quantities use the same resource:

```json
{
  "run": "L01-260810-002",
  "kind": "reject-seal",
  "quantity": 12,
  "unit": "pcs",
  "at": "2026-08-11T02:30:00+02:00"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| [`run`](../production-activities/run.md) | string | Business code of the production run that produced the quantity. | `"L01-260810-002"` |
| `kind` | string | Classification of the output quantity. See the supported kinds below. | `"good"` |
| `quantity` | number | Quantity counted in this output capture. | `1200` |
| `unit` | string | Unit in which the quantity is expressed. | `"pcs"` |
| `at` | string | Date and time when the quantity was counted, in ISO 8601 format. | `"2026-08-10T23:00:00+02:00"` |

</div>

> [!IMPORTANT]
> `run`, `kind`, and `at` together identify an output record.
>
> The referenced Run must already exist.

## Output kinds

Supported values for `kind` are:

| Kind | Meaning |
| --- | --- |
| `good` | Product accepted as good output. |
| `waste` | Product or material lost as waste. |
| `downgrade` | Product retained at a lower grade or value. |
| `reject-metal` | Product rejected by metal detection. |
| `reject-xray` | Product rejected by X-ray inspection. |
| `reject-weight` | Product rejected because of weight. |
| `reject-seal` | Product rejected because of sealing. |
| `reject-vision` | Product rejected by vision inspection. |

Reject kinds remain separate because they represent different failure modes and may require different corrective actions.

Do not combine them into a single generic reject category.

## Submit individual captures

Each Output record represents what was counted at a particular point in time.

For example:

```json
[
  {
    "run": "L01-260810-002",
    "kind": "good",
    "quantity": 1200,
    "unit": "pcs",
    "at": "2026-08-10T23:00:00+02:00"
  },
  {
    "run": "L01-260810-002",
    "kind": "reject-seal",
    "quantity": 12,
    "unit": "pcs",
    "at": "2026-08-11T02:30:00+02:00"
  }
]
```

Submit individual captures rather than shift totals or cumulative counters.

This preserves when the output occurred and avoids counting the same quantity more than once.

## Good and non-good output

Submit non-good quantities as well as good production.

For example:

```text
good          1200 pcs
reject-seal     12 pcs
```

Keeping these quantities separately allows Pulse to calculate quality performance while retaining the underlying counts.

Pre-calculated percentages should not replace the individual output quantities because percentages cannot be correctly re-aggregated across different time periods.

## Rejects

Rejects are submitted through Output rather than through a separate reject resource.

For example:

```json
{
  "run": "L01-260810-002",
  "kind": "reject-metal",
  "quantity": 1,
  "unit": "pcs",
  "at": "2026-08-10T23:07:00+02:00"
}
```

Keeping the specific reject kind allows Pulse to distinguish the source of the quality loss.

## Batching

The specification shows Output accepting multiple captures in one request:

```json
[
  {
    "run": "L01-260810-002",
    "kind": "good",
    "quantity": 1200,
    "unit": "pcs",
    "at": "2026-08-10T23:00:00+02:00"
  },
  {
    "run": "L01-260810-002",
    "kind": "reject-seal",
    "quantity": 12,
    "unit": "pcs",
    "at": "2026-08-11T02:30:00+02:00"
  }
]
```

The exact batching behavior should be verified once the implementation is available.

## API resource

| Resource | Base path |
| --- | --- |
| `Output` | `/services/pulse/food-beverage/output` |

## API methods

> [!NOTE]
> The API methods below are provisional until the Output implementation is available for verification.

### Submit output

`POST /services/pulse/food-beverage/output/insert`

Records production output.

#### Request

```http
POST /services/pulse/food-beverage/output/insert
Content-Type: application/json
```

```json
[
  {
    "run": "L01-260810-002",
    "kind": "good",
    "quantity": 1200,
    "unit": "pcs",
    "at": "2026-08-10T23:00:00+02:00"
  },
  {
    "run": "L01-260810-002",
    "kind": "reject-seal",
    "quantity": 12,
    "unit": "pcs",
    "at": "2026-08-11T02:30:00+02:00"
  }
]
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `run` | string | yes | Business code of the production run. |
| `kind` | string | yes | Output classification. |
| `quantity` | number | yes | Quantity captured. |
| `unit` | string | yes | Unit of the captured quantity. |
| `at` | string | yes | Date and time when the quantity was counted. |