# Material

Represents a raw material, component, or supply used during an operational process in Pulse.

## The Material object

```json
{
  "id": 42,
  "code": "STEEL-SHEET",
  "name": "Steel sheet",
  "measureUnit": 21
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

</div>

## API service

| Service | Base path |
| --- | --- |
| `MaterialService` | `/services/pulse/types/materials` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

Materials reference a [measure unit](measure-unit.md). Create or retrieve the measure unit before submitting the material.

## Used by

Materials are referenced by:

- [Material plans](../operational-data/material-plan.md)
- [Material usage records](../operational-data/material-usage.md)
- [Waste material usage records](../operational-data/waste-material-usage.md)