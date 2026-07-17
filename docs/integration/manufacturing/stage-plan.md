# Stage plan

Represents the planned execution time of a stage.

## The Stage plan object

```json
{
  "id": 208,
  "start": "2026-07-20T08:00:00+02:00",
  "end": "2026-07-20T10:30:00+02:00"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| [`id`](stage.md) | integer | Unique identifier of the stage plan. It is identical to the `id` of the related stage. | `208` |
| `start` | string or null | Optional planned stage start timestamp in ISO 8601 format. | `"2026-07-20T08:00:00+02:00"` |
| `end` | string or null | Optional planned stage end timestamp in ISO 8601 format. | `"2026-07-20T10:30:00+02:00"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `StagePlanService` | `/services/pulse/manufacturing/batches/stages/plan` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Stage](stage.md)

Create or retrieve the stage before submitting the stage plan.

## Referenced by

- [Material plans](material-plan.md)
- [Energy source plans](energy-source-plan.md)
- [Equipment plans](equipment-plan.md)
- [Labor plans](labor-plan.md)
- [Expense plans](expense-plan.md)