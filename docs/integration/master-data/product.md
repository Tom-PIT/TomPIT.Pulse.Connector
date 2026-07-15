# Product

Represents a finished product or other output tracked in Pulse.

## The Product object

```json
{
  "id": 42,
  "code": "CHAIR-OAK",
  "name": "Oak wood chair",
  "measureUnit": 21
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

</div>

## API service

| Service | Base path |
| --- | --- |
| `ProductService` | `/services/pulse/types/products` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

Products reference a [measure unit](measure-unit.md). Create or retrieve the measure unit before submitting the product.

## Used by

Products are referenced by [Batches](../operational-data/batch.md).