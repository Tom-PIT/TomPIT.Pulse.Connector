# Labor plan period

Represents a specific time interval during which labor is planned for a stage. A [labor plan](labor-plan.md) may contain multiple periods when the work is planned for separate intervals.

A labor plan identifies the labor role or type of work and the planned quantity. A labor plan period adds the exact timing of that planned work.

## The Labor plan period object

```json
{
  "id": 927,
  "plan": 521,
  "start": "2026-07-20T08:30:00+02:00",
  "end": "2026-07-20T10:00:00+02:00"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier of the planned labor interval. | `927` |
| [`plan`](labor-plan.md) | integer | Pulse `id` of the labor plan to which the interval belongs. | `521` |
| `start` | string | Planned start timestamp of the work, in ISO 8601 format. | `"2026-07-20T08:30:00+02:00"` |
| `end` | string | Planned end timestamp of the work, in ISO 8601 format. | `"2026-07-20T10:00:00+02:00"` |

</div>

A single [labor plan](labor-plan.md) may contain multiple periods when the work is planned for separate intervals.

## API service

| Service | Base path |
| --- | --- |
| `LaborPlanPeriodService` | `/services/pulse/manufacturing/batches/stages/plan/labor/periods` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Labor plan](labor-plan.md)

Create or retrieve the labor plan before submitting the labor plan period.