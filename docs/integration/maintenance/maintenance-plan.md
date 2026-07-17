# Maintenance plan

Describes the planned timing of a maintenance activity.

The Maintenance plan shares its identity with the related [Maintenance](maintenance.md) record.

## The Maintenance plan object

```json
{
  "id": 314,
  "start": "2026-07-20T08:00:00Z",
  "end": "2026-07-20T10:00:00Z"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| [`id`](maintenance.md) | integer | Unique identifier of the Maintenance plan. It is identical to the `id` of the related Maintenance record. | `314` |
| `start` | string | Optional planned start date and time of the maintenance activity. | `"2026-07-20T08:00:00Z"` |
| `end` | string | Optional planned end date and time of the maintenance activity. | `"2026-07-20T10:00:00Z"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `MaintenancePlanService` | `/services/pulse/maintenance/plan` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Maintenance](maintenance.md)

Create or retrieve the Maintenance record before submitting its plan.
