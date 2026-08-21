# Maintenance equipment usage

Records actual supporting equipment use for a maintenance activity.

## The Maintenance equipment usage object

```json
{
  "id": 601,
  "maintenance": 314,
  "equipment": 92,
  "quantity": 2.75,
  "price": 48.0,
  "date": "2026-07-20T09:15:00Z"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `601` |
| [`maintenance`](maintenance.md) | integer | Pulse `id` of the maintenance activity. | `314` |
| [`machine`](../master-data/machine.md) | integer | Pulse `id` of the equipment used for the maintenance activity. | `92` |
| `quantity` | number | Actual duration of equipment use, expressed in hours. | `2.75` |
| `price` | number | Optional actual price per hour. | `48.0` |
| `date` | string | Date and time when the usage occurred or was recorded. | `"2026-07-20T09:15:00Z"` |

</div>

> [!NOTE]
> When a price is based on elapsed time, Pulse expresses it per hour. Although Pulse commonly represents durations internally using ticks, hours are used for time-based price calculations.

## API resource

| Service | Base path |
| --- | --- |
| `MaintenanceEquipmentUsageService` | `/services/pulse/maintenance/usage/equipment` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Maintenance](maintenance.md)
- [Machine](../master-data/machine.md)

Create or retrieve the applicable records before submitting the maintenance equipment usage.
