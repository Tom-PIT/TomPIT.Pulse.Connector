# Labor usage

Represents the actual use of labor during a stage.

Labor usage records identify which labor role or type of work was used, how much labor was actually required, the actual price when known, and when the usage was recorded or occurred.

Pulse can compare this actual use with the related [labor plan](labor-plan.md) to identify differences in quantity, timing, and cost.

When the exact timing of the work is important, add one or more [labor usage periods](labor-usage-period.md).

## The Labor usage object

```json
{
  "id": 692,
  "stage": 208,
  "labor": 41,
  "quantity": 7.5,
  "price": 34.0,
  "date": "2026-07-20T10:48:00+02:00"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier of the labor usage record. | `692` |
| [`stage`](stage.md) | integer | Pulse `id` of the stage in which the labor was used. | `208` |
| [`labor`](../master-data/labor.md) | integer | Pulse `id` of the labor role or type of work that was used. | `41` |
| `quantity` | number | Actual quantity of labor used in hours. | `7.5` |
| `price` | number or null | Optional actual hourly price for this labor usage. | `34.0` |
| `date` | string | Date and time when the labor usage was recorded or occurred, in ISO 8601 format. | `"2026-07-20T10:48:00+02:00"` |

</div>

> [!NOTE]
> When a price is based on elapsed time, Pulse expresses it per hour. Although Pulse commonly represents durations internally using ticks, hours are used for time-based price calculations.

## API service

| Service | Base path |
| --- | --- |
| `LaborUsageService` | `/services/pulse/manufacturing/batches/stages/usage/labor` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Stage](stage.md)
- [Labor](../master-data/labor.md)

Create or retrieve the applicable records before submitting the labor usage record.

## Referenced by

- [Labor usage periods](labor-usage-period.md)