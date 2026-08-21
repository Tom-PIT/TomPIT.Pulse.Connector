# Maintenance energy source plan

Describes planned energy use for a maintenance activity.

## The Maintenance energy source plan object

```json
{
  "id": 501,
  "maintenance": 314,
  "energySource": 11,
  "quantity": 35.0,
  "price": 0.21
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `501` |
| [`maintenance`](maintenance.md) | integer | Pulse `id` of the maintenance activity. | `314` |
| [`energySource`](../master-data/energy-source.md) | integer | Pulse `id` of the energy source planned for the maintenance activity. | `11` |
| `quantity` | number | Planned quantity of the energy source. | `35.0` |
| `price` | number | Optional planned price per configured measure unit. | `0.21` |

</div>

## API resource

| Service | Base path |
| --- | --- |
| `MaintenanceEnergySourcePlanService` | `/services/pulse/maintenance/estimations/energy-sources` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Maintenance](maintenance.md)
- [Energy source](../master-data/energy-source.md)

Create or retrieve the applicable records before submitting the maintenance energy source plan.
