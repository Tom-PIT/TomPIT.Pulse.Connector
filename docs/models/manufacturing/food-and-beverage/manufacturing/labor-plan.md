# Labor plan

Represents the planned use of labor during a stage.

A stage can require several labor roles or types of work. Labor plans allow Pulse to understand what kind of labor is expected, how much is planned, and the expected cost.

When the exact timing of the planned work is important, add one or more [labor plan periods](labor-plan-period.md).

## The Labor plan object

```json
{
  "id": 521,
  "stage": 208,
  "labor": 41,
  "quantity": 6.0,
  "price": 32.0
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier of the labor plan. | `521` |
| [`stage`](stage.md) | integer | Pulse `id` of the stage for which the labor is planned. | `208` |
| [`labor`](../master-data/labor.md) | integer | Pulse `id` of the planned labor role or type of work. | `41` |
| `quantity` | number | Planned quantity of labor in hours. | `6.0` |
| `price` | number or null | Optional planned hourly price. | `32.0` |

</div>

> [!NOTE]
> When a price is based on elapsed time, Pulse expresses it per hour. Although Pulse commonly represents durations internally using ticks, hours are used for time-based price calculations.

## API service

| Service | Base path |
| --- | --- |
| `LaborPlanService` | `/services/pulse/manufacturing/batches/stages/plan/labor` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Stage](stage.md)
- [Labor](../master-data/labor.md)

Create or retrieve the applicable records before submitting the labor plan.

## Referenced by

- [Labor plan periods](labor-plan-period.md)