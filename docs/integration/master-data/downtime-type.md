# Downtime type

Defines a planned or unplanned type of downtime in Pulse.

## The Downtime type object

```json
{
  "id": 9,
  "category": 7,
  "code": "MAINTENANCE",
  "name": "Planned maintenance",
  "kind": 1
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `9` |
| [`category`](downtime-category.md) | integer | Pulse `id` of the category used to group the downtime type. | `7` |
| `code` | string | Business code used to identify the downtime type in external systems and integrations. | `"MAINTENANCE"` |
| `name` | string | Human-readable name of the downtime type. | `"Planned maintenance"` |
| `kind` | enum | Classification of the downtime as unplanned (`0`) or planned (`1`). | `1` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `DowntimeTypeService` | `/services/pulse/types/downtime-types` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

Downtime types reference a [downtime category](downtime-category.md). Create or retrieve the category before submitting the downtime type.
