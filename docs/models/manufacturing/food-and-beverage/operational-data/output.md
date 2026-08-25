# Output

Represents a quantity produced during a production run.

Output records both usable production and quantities that did not become good product, including waste, downgraded product, and specific reject types.

## The Output object

```json
{
  "run": "L03-260810-002",
  "quantity": 1200,
  "kind": "good",
  "at": "2026-08-10T23:00:00+02:00"
}
```

A rejected quantity is submitted through the same resource:

```json
{
  "run": "L03-260810-002",
  "quantity": 3,
  "kind": "reject-weight",
  "at": "2026-08-10T23:04:00+02:00"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| [`run`](run.md) | string | Code of the production run that produced the quantity. | `"L03-260810-002"` |
| `quantity` | number | Quantity captured for this output record. | `1200` |
| `kind` | string | Classification of the produced quantity. | `"good"` |
| `at` | string | Timestamp of the capture, in ISO 8601 format with an explicit offset. | `"2026-08-10T23:00:00+02:00"` |

</div>

## Output kinds

The following values are supported for `kind`:

| Kind | Meaning |
| --- | --- |
| `good` | Product accepted as good output. |
| `waste` | Product or material lost as waste. |
| `downgrade` | Product retained at a lower grade or value. |
| `reject-metal` | Product rejected by metal detection. |
| `reject-weight` | Product rejected because of weight. |
| `reject-seal` | Product rejected because of sealing. |
| `reject-fill` | Product rejected because of filling. |
| `reject-label` | Product rejected because of labelling. |

The reject kinds remain separate because they represent different failure modes.

Do not collapse them into a single generic `reject` value.

## Good and total output

Submit all produced quantities, not only good output.

For example:

```json
[
  {
    "run": "L03-260810-002",
    "quantity": 1200,
    "kind": "good",
    "at": "2026-08-10T23:00:00+02:00"
  },
  {
    "run": "L03-260810-002",
    "quantity": 3,
    "kind": "reject-weight",
    "at": "2026-08-10T23:04:00+02:00"
  }
]
```

Keeping good and non-good quantities separately allows Pulse to calculate quality rates while retaining the underlying quantities.

This is preferable to submitting a pre-calculated quality percentage because the absolute quantities can be aggregated correctly across different periods.

## Capture individual output

Submit individual output captures rather than accumulated totals.

For example, if the source system records production by pallet, submit one record for each pallet rather than repeatedly submitting the running total for the shift.

Likewise, a reject counter should submit the quantity associated with the capture rather than the cumulative detector count.

This prevents the same production quantity from being counted more than once.

## Rejects

Rejects use `/output`; there is no separate reject resource.

For example:

```json
{
  "run": "L03-260810-002",
  "quantity": 1,
  "kind": "reject-metal",
  "at": "2026-08-10T23:07:00+02:00"
}
```

Keeping the specific reject kind allows Pulse to distinguish different sources of quality loss instead of treating all rejected product as one category.

## Batching

The output resource accepts either one object or an array of objects.

For example:

```json
[
  {
    "run": "L03-260810-002",
    "quantity": 1200,
    "kind": "good",
    "at": "2026-08-10T23:00:00+02:00"
  },
  {
    "run": "L03-260810-002",
    "quantity": 3,
    "kind": "reject-weight",
    "at": "2026-08-10T23:04:00+02:00"
  }
]
```

This allows a live source to submit captures individually and a historian or file-based integration to submit several captures together.

## API resource

| Resource | Base path |
| --- | --- |
| Output | `/services/pulse/food-beverage/output` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Run](run.md)

The referenced run must be available before submitting output records.