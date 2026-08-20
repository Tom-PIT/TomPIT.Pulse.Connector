# Product

Represents a finished product or other output tracked in Pulse.

## The Product object

```json
{
  "id": 42,
  "code": "YOG-STRAWBERRY-150G",
  "name": "Strawberry Yogurt 150 g",
  "measureUnit": 21,
  "price": 0.79,
  "description": "Strawberry yogurt packaged in a 150 g cup"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `21` |
| `code` | string | Unique business code within the entity type, used for external identification and integrations. | `"YOG-STRAWBERRY-150G"` |
| `name` | string | Human-readable name of the product. | `"Strawberry Yogurt 150 g"` |
| [`measureUnit`](measure-unit.md) | integer | Pulse `id` of the measure unit used for the product. | `21` |
| `price` | number or null | Optional default price per measure unit. Pulse may use this value when a related operational record does not provide its own price. | `0.79` |
| `description` | string or null | Optional description of the product or output tracked in Pulse. | `"Strawberry yogurt packaged in a 150 g cup"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `ProductService` | `/services/pulse/types/products` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Measure unit](measure-unit.md)

Create or retrieve the measure unit before submitting the product.

## Referenced by

- [Batches](../manufacturing/batch.md)