# Maintenance labor plan

Describes planned labor use for a maintenance activity.

## The Maintenance labor plan object

```json
{
  "id": 501,
  "maintenance": 314,
  "labor": 24,
  "quantity": 3.0,
  "price": 42.0
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `501` |
| [`maintenance`](maintenance.md) | integer | Pulse `id` of the maintenance activity. | `314` |
| [`labor`](../master-data/labor.md) | integer | Pulse `id` of the labor planned for the maintenance activity. | `24` |
| `quantity` | number | Planned duration of labor, expressed in hours. | `3.0` |
| `price` | number | Optional planned price per hour. | `42.0` |

</div>

> [!NOTE]
> When a price is based on elapsed time, Pulse expresses it per hour. Although Pulse commonly represents durations internally using ticks, hours are used for time-based price calculations.

## API service

| Service | Base path |
| --- | --- |
| `MaintenanceLaborPlanService` | `/services/pulse/maintenance/plan/labor` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Maintenance](maintenance.md)
- [Labor](../master-data/labor.md)

Create or retrieve the applicable records before submitting the maintenance labor plan.
