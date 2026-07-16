# Ambient type

Defines a measurement type, its unit, and expected value range in Pulse.

## The Ambient type object

```json
{
  "id": 22,
  "code": "TEMPERATURE",
  "name": "Temperature",
  "description": "Ambient temperature in the production area",
  "measureUnit": 4,
  "min": "18",
  "max": "26",
  "expected": "22"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `22` |
| `code` | string | Business code used to identify the ambient type in external systems and integrations. | `"TEMPERATURE"` |
| `name` | string | Human-readable name of the ambient type. | `"Temperature"` |
| `description` | string or null | Additional information about the ambient type. | `"Ambient temperature in the production area"` |
| [`measureUnit`](measure-unit.md) | integer or null | Pulse `id` of the measure unit used for the ambient values. | `4` |
| `min` | string or null | Minimum expected value for the ambient measurement. | `"18"` |
| `max` | string or null | Maximum expected value for the ambient measurement. | `"26"` |
| `expected` | string or null | Expected value for the ambient measurement. | `"22"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `AmbientTypeService` | `/services/pulse/types/ambient-types` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

Ambient types may reference a [measure unit](measure-unit.md). Create or retrieve the measure unit before submitting an ambient type that uses one.

## Used by

Ambient types are referenced by [Ambient value records](../manufacturing/ambient-value.md).
