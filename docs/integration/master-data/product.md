# Product

Represents a finished product or other output tracked in Pulse.

## The Product object

```json
{
  "Id": 42,
  "Code": "CHAIR-OAK",
  "Name": "Oak wood chair",
  "MeasureUnit": 21
}
```

## Attributes

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `Id` | integer | Unique identifier assigned by Pulse. | `42` |
| `Code` | string | Unique business code within the entity type, used for external identification and integrations. | `"CHAIR-OAK"` |
| `Name` | string | Human-readable name of the product. | `"Oak wood chair"` |
| [`MeasureUnit`](measure-unit.md) | integer | Pulse `Id` of the measure unit used for the product. | `21` |

## API service

| Service | Base path |
| --- | --- |
| `ProductService` | `/services/pulse/types/products` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

Products reference a [measure unit](measure-unit.md). Create or retrieve the measure unit before submitting the product.

## Used by

Products are referenced by [Batches](../operational-data/batch.md).