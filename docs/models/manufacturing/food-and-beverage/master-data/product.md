# Product

Represents a finished product or other output tracked in Pulse.

## The Product object

```json
{
  "code": "YOG-STRAWBERRY-150G",
  "name": "Strawberry Yogurt 150 g",
  "brand": "BRAND-A",
  "measureUnit": "pcs",
  "unitPrice": 0.79
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the product in source systems and integrations. | `"YOG-STRAWBERRY-150G"` |
| `name` | string | Human-readable name of the product. | `"Strawberry Yogurt 150 g"` |
| `brand` | string or null | Optional code identifying the product brand. | `"BRAND-A"` |
| `measureUnit` | string | Code of the measure unit used for the product. | `"pcs"` |
| `unitPrice` | number or null | Optional price per product unit. | `0.79` |

</div>

## API resource

| Resource | Base path |
| --- | --- |
| Product | `/services/pulse/food-beverage/products` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Measure unit](measure-unit.md)

The measure unit referenced by `measureUnit` must be available before submitting the product.

## Referenced by

- [Expected values](../expected.md)