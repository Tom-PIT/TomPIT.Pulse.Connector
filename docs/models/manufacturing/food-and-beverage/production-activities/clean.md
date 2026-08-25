# Clean

Represents a cleaning activity on a production line.

A clean records the time and context of cleaning work such as a rinse, wet clean, full CIP, or allergen clean.

The products produced before and after the clean can be recorded so Pulse can analyse cleaning as a production transition rather than only as downtime.

## The Clean object

```json
{
  "code": "CIP-260811-014",
  "line": "L03",
  "regime": "full-cip",
  "after": "SKU-4471",
  "before": "SKU-2210",
  "at": "2026-08-11T06:00:00+02:00"
}
```

When the clean finishes, submit the same `code` with its end time:

```json
{
  "code": "CIP-260811-014",
  "end": "2026-08-11T07:45:00+02:00"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the cleaning activity in source systems and integrations. | `"CIP-260811-014"` |
| [`line`](../master-data/production-line.md) | string | Code of the production line being cleaned. | `"L03"` |
| [`regime`](../master-data/clean-regime.md) | string | Code of the cleaning regime used. | `"full-cip"` |
| [`after`](../master-data/product.md) | string or null | Optional code of the product produced before the clean. | `"SKU-4471"` |
| [`before`](../master-data/product.md) | string or null | Optional code of the product intended after the clean. | `"SKU-2210"` |
| `at` | string | Timestamp when the clean started, in ISO 8601 format with an explicit offset. | `"2026-08-11T06:00:00+02:00"` |
| `end` | string or null | Timestamp when the clean ended, in ISO 8601 format with an explicit offset. | `"2026-08-11T07:45:00+02:00"` |

</div>

## Clean lifecycle

A clean can be submitted when cleaning starts:

```json
{
  "code": "CIP-260811-014",
  "line": "L03",
  "regime": "full-cip",
  "after": "SKU-4471",
  "before": "SKU-2210",
  "at": "2026-08-11T06:00:00+02:00"
}
```

When cleaning finishes, submit the fields that became known:

```json
{
  "code": "CIP-260811-014",
  "end": "2026-08-11T07:45:00+02:00"
}
```

Fields omitted from the second request remain unchanged.

## Product transitions

The `after` and `before` fields describe the product transition surrounding the clean.

```text
SKU-4471
   │
   │ clean
   ▼
SKU-2210
```

This allows Pulse to distinguish between different cleaning requirements and compare how long similar product transitions actually take.

For example, changing from one product to another may require a full CIP, while the reverse transition may require only a rinse.

Without the product transition, the clean can still be recorded, but Pulse has less context for analysing sequencing and cleaning performance.

## Expected duration

Expected cleaning duration can be declared through [Expected values](../expected.md) for a clean regime and, when applicable, a specific product transition.

Pulse can then compare the expected duration with the actual duration recorded by the clean.

## Consumption

Water, chemicals, steam, labor, and other resources used during cleaning are recorded separately through [consumption](consumption.md).

For example:

```json
{
  "clean": "CIP-260811-014",
  "category": "chemical",
  "item": "CAUSTIC-01",
  "quantity": 18,
  "unit": "kg",
  "at": "2026-08-11T06:20:00+02:00"
}
```

This keeps the cleaning activity itself separate from the resources consumed while performing it.

## API resource

| Resource | Base path |
| --- | --- |
| Clean | `/services/pulse/food-beverage/cleans` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Production line](../master-data/production-line.md)
- [Clean regime](../master-data/clean-regime.md)
- [Product](../master-data/product.md), when `after` or `before` is provided

The referenced records must be available before submitting the clean.