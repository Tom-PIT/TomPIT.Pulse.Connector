# Measure unit

Represents a unit used to express quantities and measured values consistently throughout Pulse.

## The Measure unit object

```json
{
  "id": 6,
  "code": "kg",
  "name": "Kilogram"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `6` |
| `code` | string | Unique business code within the entity type, used for external identification and integrations. | `"kg"` |
| `name` | string | Human-readable name of the measure unit. | `"Kilogram"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `MeasureUnitService` | `/services/pulse/types/measure-units` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Used by

Measure units are referenced by:

- [Products](product.md)
- [Materials](material.md)
- [Energy sources](energy-source.md)
- [Ambient types](ambient-type.md)
