# Maintenance equipment plan

Describes planned supporting equipment use for a maintenance activity.

## The Maintenance equipment plan object

```json
{
  "id": 501,
  "maintenance": 314,
  "equipment": 92,
  "quantity": 2.5,
  "price": 48.0
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `501` |
| [`maintenance`](maintenance.md) | integer | Pulse `id` of the maintenance activity. | `314` |
| [`machine`](../master-data/machine.md) | integer | Pulse `id` of the equipment planned for the maintenance activity. | `92` |
| `quantity` | number | Planned duration of equipment use, expressed in hours. | `2.5` |
| `price` | number | Optional planned price per hour. | `48.0` |

</div>

> [!NOTE]
> When a price is based on elapsed time, Pulse expresses it per hour. Although Pulse commonly represents durations internally using ticks, hours are used for time-based price calculations.

## API resource

| Resource | Base path |
| --- | --- |
| Plant | `/services/pulse/food-beverage/plants` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Maintenance](maintenance.md)
- [Machine](../master-data/machine.md)

Create or retrieve the applicable records before submitting the maintenance equipment plan.
