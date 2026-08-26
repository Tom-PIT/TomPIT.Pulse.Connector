# Output

Records quantities produced during a production [Run](../production-activities/run.md).

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
| [`run`](../production-activities/run.md) | string | Business code of the production Run that produced the quantity. | `"L01-260810-002"` |
| `kind` | string | Classification of the output quantity. See the supported kinds below. | `"good"` |
| `quantity` | number | Quantity counted in this Output capture. | `1200` |
| `unit` | string | Unit in which the quantity is expressed. Must match the configured output Measurement unit. | `"pcs"` |
| `at` | string | Date and time when the quantity was counted, in ISO 8601 format. | `"2026-08-10T23:00:00+02:00"` |

</div>

> [!IMPORTANT]
> `run`, `kind`, and `at` together identify an Output record.
>
> The referenced Run must already exist.
>
> `unit` must match the unit configured for the output Measurements.

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

Reject kinds remain separate because they represent different failure modes.

Do not combine them into a single generic reject category.

## Individual captures

Each Output record represents a quantity counted at a specific point in time.

For example, two separate captures are submitted as two separate requests:

```json
{
  "run": "L01-260810-002",
  "kind": "good",
  "quantity": 1200,
  "unit": "pcs",
  "at": "2026-08-10T23:00:00+02:00"
}
```

and:

```json
{
  "run": "L01-260810-002",
  "kind": "reject-seal",
  "quantity": 12,
  "unit": "pcs",
  "at": "2026-08-11T02:30:00+02:00"
}
```

Submit individual captures rather than cumulative counters where possible.

This preserves when the output occurred and avoids counting the same production more than once.

## Good and non-good output

Submit non-good quantities as well as good production.

For example:

```text
good          1200 pcs
reject-seal     12 pcs
```

Keeping these quantities separate allows Pulse to distinguish good production from individual loss categories while retaining the underlying counts.

Pre-calculated percentages should not replace the individual output quantities because percentages cannot be reliably re-aggregated across different time periods.

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

## Corrections

An Output record is identified by:

```text
run + kind + at
```

Use `update` or `patch` to correct an existing capture.

For example, if a quantity originally recorded as `1200` should have been `1215`, keep the same `run`, `kind`, and `at` and update the quantity.

The identity fields themselves are not changed by PATCH.

## Reference protection

A Run referenced by an existing Output record cannot be deleted until the Output reference is removed.

## API resource

| Resource | Base path |
| --- | --- |
| `Output` | `/services/pulse/food-beverage/output` |

## API methods

### Submit output

`POST /services/pulse/food-beverage/output/insert`

Records one production Output capture.

#### Request

```http
POST /services/pulse/food-beverage/output/insert
Content-Type: application/json
```

```json
{
  "run": "L01-260810-002",
  "kind": "good",
  "quantity": 1200,
  "unit": "pcs",
  "at": "2026-08-10T23:00:00+02:00"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `run` | string | yes | Business code of the production Run. |
| `kind` | string | yes | Output classification. |
| `quantity` | number | yes | Quantity captured. |
| `unit` | string | yes | Unit of the captured quantity. Must match the configured output Measurement unit. |
| `at` | string | yes | Date and time when the quantity was counted. |

### Update output

`PUT /services/pulse/food-beverage/output/update`

Updates an existing Output record.

The record is identified by `run`, `kind`, and `at`.

#### Request

```http
PUT /services/pulse/food-beverage/output/update
Content-Type: application/json
```

```json
{
  "run": "L01-260810-002",
  "kind": "good",
  "quantity": 1215,
  "unit": "pcs",
  "at": "2026-08-10T23:00:00+02:00"
}
```

### Patch output

`PATCH /services/pulse/food-beverage/output/patch`

Partially updates an existing Output record.

The record is identified by `properties.run`, `properties.kind`, and `properties.at`. All three are required.

Only the correctable values such as `quantity` and `unit` are changed. The composite key remains unchanged.

#### Request

```http
PATCH /services/pulse/food-beverage/output/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "run": "L01-260810-002",
    "kind": "good",
    "at": "2026-08-10T23:00:00+02:00",
    "quantity": 1215
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.run` | string | yes | Run business code identifying the Output record. |
| `properties.kind` | string | yes | Output kind identifying the Output record. |
| `properties.at` | string | yes | Output timestamp identifying the Output record. |
| `properties.quantity` | number | no | New output quantity. |
| `properties.unit` | string | no | New output unit. Must match the configured output Measurement unit. |

### Retrieve output

`GET /services/pulse/food-beverage/output/select`

Returns the Output record identified by its composite business key.

#### Request

```http
GET /services/pulse/food-beverage/output/select?run=L01-260810-002&kind=good&at=2026-08-10T23:00:00%2B02:00
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `run` | string | yes | Business code of the production Run. |
| `kind` | string | yes | Output classification. |
| `at` | string | yes | Date and time of the Output capture. |

#### Example response

```json
{
  "run": "L01-260810-002",
  "kind": "good",
  "quantity": 1200,
  "unit": "pcs",
  "at": "2026-08-10T23:00:00+02:00"
}
```

### List output

`GET /services/pulse/food-beverage/output/query`

Returns Output records matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/output/query?runs=L01-260810-002&kinds=good
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `runs` | string or array of strings | no | Limits results to Output from the specified Runs. |
| `kinds` | string or array of strings | no | Limits results to the specified Output kinds. |
| `from` | string | no | Limits results to Output captured at or after the specified date and time. |
| `to` | string | no | Limits results to Output captured at or before the specified date and time. |

Multiple values can be supplied by repeating the query parameter:

```http
GET /services/pulse/food-beverage/output/query?kinds=good&kinds=reject-seal
```

### Delete output

`DELETE /services/pulse/food-beverage/output/delete`

Deletes the Output record identified by its composite business key.

#### Request

```http
DELETE /services/pulse/food-beverage/output/delete?run=L01-260810-002&kind=good&at=2026-08-10T23:00:00%2B02:00
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `run` | string | yes | Business code of the production Run. |
| `kind` | string | yes | Output classification. |
| `at` | string | yes | Date and time of the Output capture. |