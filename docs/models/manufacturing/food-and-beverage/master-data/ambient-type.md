# Ambient type

Defines a measurement type, its unit, and expected value range in Pulse.

## The Ambient type object

```json
{
  "id": 22,
  "code": "PRODUCT-TEMPERATURE",
  "name": "Product Temperature",
  "description": "Product temperature measured during processing",
  "measureUnit": 4,
  "min": "72",
  "max": "75",
  "expected": "73"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `22` |
| `code` | string | Business code used to identify the ambient type in external systems and integrations. | `"PRODUCT-TEMPERATURE"` |
| `name` | string | Human-readable name of the ambient type. | `"Product Temperature"` |
| `description` | string or null | Additional information about the ambient type. | `"Product temperature measured during processing"` |
| [`measureUnit`](measure-unit.md) | integer or null | Pulse `id` of the measure unit used for the ambient values. | `4` |
| `min` | string or null | Minimum expected value for the ambient measurement. | `"72"` |
| `max` | string or null | Maximum expected value for the ambient measurement. | `"75"` |
| `expected` | string or null | Expected value for the ambient measurement. | `"73"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `AmbientTypeService` | `/services/pulse/types/ambient-types` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Measure unit](measure-unit.md)

Create or retrieve the measure unit before submitting an ambient type that uses one.

## Referenced by

- [Ambient value records](../manufacturing/ambient-value.md)
