# Product

Represents a finished product or other output tracked in Pulse.

## The Product object

```json
{
  "id": 42,
  "code": "CHAIR-OAK",
  "name": "Oak wood chair",
  "measureUnit": 21,
  "price": 49.99,
  "description": "Finished oak chair produced for sale"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `42` |
| `code` | string | Unique business code within the entity type, used for external identification and integrations. | `"CHAIR-OAK"` |
| `name` | string | Human-readable name of the product. | `"Oak wood chair"` |
| [`measureUnit`](measure-unit.md) | integer | Pulse `id` of the measure unit used for the product. | `21` |
| `price` | number or null | Optional default price per measure unit. Pulse may use this value when a related operational record does not provide its own price. | `49.99` |
| `description` | string or null | Optional description of the product or output tracked in Pulse. | `"Finished oak chair produced for sale"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `ProductService` | `/services/pulse/types/products` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Measure unit](measure-unit.md)

Create or retrieve the measure unit before submitting the product.

## Referenced by

- [Batches](../manufacturing/batch.md)