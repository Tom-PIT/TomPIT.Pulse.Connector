# Material

Represents a raw material, component, or supply used during an operational process in Pulse.

## The Material object

```json
{
  "id": 42,
  "code": "STEEL-SHEET",
  "name": "Steel sheet",
  "measureUnit": 21,
  "price": 12.50,
  "description": "Steel sheet used as an input material in production"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `42` |
| `code` | string | Unique business code within the entity type, used for external identification and integrations. | `"STEEL-SHEET"` |
| `name` | string | Human-readable name of the material. | `"Steel sheet"` |
| [`measureUnit`](measure-unit.md) | integer | Pulse `id` of the measure unit in which material quantities are expressed. | `21` |
| `price` | number or null | Optional default price per measure unit. Pulse may use this value when a related operational record does not provide its own price. | `12.50` |
| `description` | string or null | Optional description of the material and its role in the process. | `"Steel sheet used as an input material in production"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `MaterialService` | `/services/pulse/types/materials` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Measure unit](measure-unit.md)

Create or retrieve the measure unit before submitting the material.

## Referenced by

- [Material plans](../manufacturing/material-plan.md)
- [Material usage records](../manufacturing/material-usage.md)
- [Waste material usage records](../manufacturing/waste-material-usage.md)