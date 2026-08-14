# Maintenance energy source usage

Records actual energy use for a maintenance activity.

## The Maintenance energy source usage object

```json
{
  "id": 601,
  "maintenance": 314,
  "energySource": 11,
  "quantity": 41.0,
  "price": 0.22,
  "date": "2026-07-20T09:15:00Z",
  "lot": 73
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `601` |
| [`maintenance`](maintenance.md) | integer | Pulse `id` of the maintenance activity. | `314` |
| [`energySource`](../master-data/energy-source.md) | integer | Pulse `id` of the energy source used for the maintenance activity. | `11` |
| `quantity` | number | Actual quantity of the energy source used. | `41.0` |
| `price` | number | Optional actual price per configured measure unit. | `0.22` |
| `date` | string | Date and time when the usage occurred or was recorded. | `"2026-07-20T09:15:00Z"` |
| [`lot`](../traceability/lot.md) | integer | Optional Pulse `id` of the traceable lot associated with the usage. | `73` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `MaintenanceEnergySourceUsageService` | `/services/pulse/maintenance/usage/energy-sources` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Maintenance](maintenance.md)
- [Energy source](../master-data/energy-source.md)

May also reference:

- [Lot](../traceability/lot.md)

Create or retrieve the applicable records before submitting the maintenance energy source usage.
