# Downtime category

Represents a category used to group related downtime types in Pulse.

## The Downtime category object

```json
{
  "id": 7,
  "code": "UNPLANNED",
  "name": "Unplanned downtime"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `7` |
| `code` | string | Business code used to identify the downtime category in external systems and integrations. | `"UNPLANNED"` |
| `name` | string | Human-readable name of the downtime category. | `"Unplanned downtime"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `DowntimeCategoryService` | `/services/pulse/types/downtime-categories` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Referenced by

- [Downtime types](downtime-type.md)