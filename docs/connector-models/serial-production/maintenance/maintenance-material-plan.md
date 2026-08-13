# Maintenance material plan

Describes planned material use for a maintenance activity.

## The Maintenance material plan object

```json
{
  "id": 501,
  "maintenance": 314,
  "material": 62,
  "quantity": 4.0,
  "price": 18.5
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `501` |
| [`maintenance`](maintenance.md) | integer | Pulse `id` of the maintenance activity. | `314` |
| [`material`](../master-data/material.md) | integer | Pulse `id` of the material planned for the maintenance activity. | `62` |
| `quantity` | number | Planned quantity of material. | `4.0` |
| `price` | number | Optional planned price per configured measure unit. | `18.5` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `MaintenanceMaterialPlanService` | `/services/pulse/maintenance/plan/materials` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Maintenance](maintenance.md)
- [Material](../master-data/material.md)

Create or retrieve the applicable records before submitting the maintenance material plan.
