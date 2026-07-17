# Stage

Represents an operation or execution step within a batch.

A stage divides a batch into meaningful parts of the process so that plans, actual usage, delays, downtime, waste, measurements, and other operational records can be linked to the part of the activity where they occurred.

## The Stage object

```json
{
  "id": 208,
  "batch": 105,
  "code": "PACKING",
  "name": "Packing"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `208` |
| [`batch`](batch.md) | integer | Pulse `id` of the batch to which the stage belongs. | `105` |
| `code` | string | Business code used to identify the stage in source systems and integrations. Stable codes help Pulse compare equivalent stages across batches. | `"PACKING"` |
| `name` | string | Human-readable name of the stage. | `"Packing"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `StageService` | `/services/pulse/manufacturing/batches/stages` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Batch](batch.md)

Create or retrieve the batch before submitting the stage.

## Referenced by

- [Stage plans](stage-plan.md)
- [Stage usage records](stage-usage.md)
- [Stage delay records](stage-delay.md)
- [Downtime records](downtime.md)
- [Material plans](material-plan.md)
- [Material usage records](material-usage.md)
- [Energy source plans](energy-source-plan.md)
- [Energy source usage records](energy-source-usage.md)
- [Equipment plans](equipment-plan.md)
- [Equipment usage records](equipment-usage.md)
- [Labor plans](labor-plan.md)
- [Labor usage records](labor-usage.md)
- [Expense plans](expense-plan.md)
- [Expense usage records](expense-usage.md)
- [Waste records](waste.md)
- [Ambient value records](ambient-value.md)