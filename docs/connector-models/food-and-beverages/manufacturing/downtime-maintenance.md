# Downtime maintenance

Represents the relationship between a downtime record and a maintenance activity.

A downtime record explains where and why operational time was lost. A maintenance record describes the work performed, including its planned and actual resource use. Downtime maintenance connects these events so that Pulse can attribute the applicable share of a maintenance activity and its cost to a specific downtime event.

## The Downtime maintenance object

```json
{
  "id": 428,
  "downtime": 146,
  "maintenance": 314,
  "percentage": 0.25
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier of the relationship between the downtime and maintenance records. | `428` |
| [`downtime`](downtime.md) | integer | Pulse `id` of the downtime record associated with the maintenance activity. | `146` |
| [`maintenance`](../maintenance/maintenance.md) | integer | Pulse `id` of the maintenance activity associated with the downtime. | `314` |
| `percentage` | number | Share of the downtime attributed to the maintenance activity, expressed as a decimal fraction between `0` and `1`. | `0.25` |

</div>

> [!NOTE]
> Submit percentages as decimal fractions. For example, submit **25%** as `0.25` and **100%** as `1`.

## Cost allocation

A maintenance activity is not always caused entirely by one downtime event.

For example, a maintenance technician may resolve the immediate problem and also perform preventive inspection, cleaning, adjustment, or additional replacement work. A single maintenance activity may also relate to several downtime events.

`percentage` defines how much of the maintenance activity is attributed to the specific downtime record:

- `1.00` attributes the entire maintenance activity to the downtime.
- `0.25` attributes one quarter of the maintenance activity to the downtime.
- A value between `0` and `1.00` attributes the corresponding proportional share.

Pulse can use this percentage when assigning maintenance costs to the downtime. If a maintenance activity costs 300 EUR and `percentage` is `0.25`, the downtime is assigned 75 EUR of that maintenance cost.

> [!NOTE]
> Downtime maintenance does not replace the related downtime or maintenance records. It only defines the relationship between them and the share attributed to the downtime.

## API service

| Service | Base path |
| --- | --- |
| `DowntimeMaintenanceService` | `/services/pulse/manufacturing/batches/stages/downtime/maintenance` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Downtime](downtime.md)
- [Maintenance](../maintenance/maintenance.md)

Create or retrieve the applicable records before submitting the downtime maintenance record.