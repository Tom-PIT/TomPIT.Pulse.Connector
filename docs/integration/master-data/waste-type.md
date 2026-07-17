# Waste type

Defines a classification for waste in Pulse.

## The Waste type object

```json
{
  "id": 16,
  "code": "SCRAP",
  "name": "Production scrap",
  "description": "Material rejected or discarded during production"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `16` |
| `code` | string | Business code used to identify the waste type in external systems and integrations. | `"SCRAP"` |
| `name` | string | Human-readable name of the waste type. | `"Production scrap"` |
| `description` | string or null | Optional description of the waste classification. | `"Material rejected or discarded during production"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `WasteTypeService` | `/services/pulse/types/waste-types` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Referenced by

- [Waste records](../manufacturing/waste.md)