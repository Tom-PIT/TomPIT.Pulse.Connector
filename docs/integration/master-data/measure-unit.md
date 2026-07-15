# Measure unit

Represents a unit used to express quantities and measured values consistently throughout Pulse.

## The Measure unit object

```json
{
  "Id": 6,
  "Code": "kg",
  "Name": "Kilogram"
}
```

## Attributes

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `Id` | integer | Unique identifier assigned by Pulse. | `6` |
| `Code` | string | Unique business code within the entity type, used for external identification and integrations. | `"kg"` |
| `Name` | string | Human-readable name of the measure unit. | `"Kilogram"` |

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
