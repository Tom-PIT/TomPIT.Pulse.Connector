# Maintenance usage

Describes the actual timing of a maintenance activity.

The Maintenance usage record shares its identity with the related [Maintenance](maintenance.md) record.

## The Maintenance usage object

```json
{
  "id": 314,
  "start": "2026-07-20T08:12:00Z",
  "end": "2026-07-20T10:26:00Z"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| [`id`](maintenance.md) | integer | Unique identifier of the Maintenance usage record. It is identical to the `id` of the related Maintenance record. | `314` |
| `start` | string | Optional actual start date and time of the maintenance activity. | `"2026-07-20T08:12:00Z"` |
| `end` | string | Optional actual end date and time of the maintenance activity. | `"2026-07-20T10:26:00Z"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `MaintenanceUsageService` | `/services/pulse/maintenance/usage` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Maintenance](maintenance.md)

Create or retrieve the Maintenance record before submitting its usage.