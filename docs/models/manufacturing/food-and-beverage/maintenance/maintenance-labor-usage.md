# Maintenance labor usage

Records actual labor use for a maintenance activity.

## The Maintenance labor usage object

```json
{
  "id": 601,
  "maintenance": 314,
  "labor": 24,
  "quantity": 3.5,
  "price": 42.0,
  "date": "2026-07-20T09:15:00Z"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `601` |
| [`maintenance`](maintenance.md) | integer | Pulse `id` of the maintenance activity. | `314` |
| [`labor`](../master-data/labor.md) | integer | Pulse `id` of the labor used for the maintenance activity. | `24` |
| `quantity` | number | Actual duration of labor, expressed in hours. | `3.5` |
| `price` | number | Optional actual price per hour. | `42.0` |
| `date` | string | Date and time when the usage occurred or was recorded. | `"2026-07-20T09:15:00Z"` |

</div>

> [!NOTE]
> When a price is based on elapsed time, Pulse expresses it per hour. Although Pulse commonly represents durations internally using ticks, hours are used for time-based price calculations.

## API resource

| Service | Base path |
| --- | --- |
| `MaintenanceLaborUsageService` | `/services/pulse/maintenance/usage/labor` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Maintenance](maintenance.md)
- [Labor](../master-data/labor.md)

Create or retrieve the applicable records before submitting the maintenance labor usage.
