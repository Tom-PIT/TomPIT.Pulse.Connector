# Maintenance material usage

Records actual material use for a maintenance activity.

## The Maintenance material usage object

```json
{
  "id": 601,
  "maintenance": 314,
  "material": 62,
  "quantity": 5.0,
  "price": 18.75,
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
| [`material`](../master-data/material.md) | integer | Pulse `id` of the material used for the maintenance activity. | `62` |
| `quantity` | number | Actual quantity of material used. | `5.0` |
| `price` | number | Optional actual price per configured measure unit. | `18.75` |
| `date` | string | Date and time when the usage occurred or was recorded. | `"2026-07-20T09:15:00Z"` |
| [`lot`](../traceability/lot.md) | integer | Optional Pulse `id` of the traceable lot associated with the usage. | `73` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `MaintenanceMaterialUsageService` | `/services/pulse/maintenance/usage/materials` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Maintenance](maintenance.md)
- [Material](../master-data/material.md)

May also reference:

- [Lot](../traceability/lot.md)

Create or retrieve the applicable records before submitting the maintenance material usage.
