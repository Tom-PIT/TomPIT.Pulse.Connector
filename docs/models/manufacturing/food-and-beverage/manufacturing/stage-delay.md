# Stage delay

Represents a delay associated with a stage during a specific time interval.

## The Stage delay object

```json
{
  "id": 522,
  "stage": 208,
  "delay": 19,
  "start": "2026-07-20T09:10:00+02:00",
  "end": "2026-07-20T09:35:00+02:00"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `522` |
| [`stage`](stage.md) | integer | Pulse `id` of the stage associated with the delay. | `208` |
| [`delay`](../master-data/delay.md) | integer | Pulse `id` of the delay type. | `19` |
| `start` | string or null | Optional start timestamp of the delay in ISO 8601 format. | `"2026-07-20T09:10:00+02:00"` |
| `end` | string or null | Optional end timestamp of the delay in ISO 8601 format. | `"2026-07-20T09:35:00+02:00"` |

</div>

## API resource

| Service | Base path |
| --- | --- |
| `StageDelayService` | `/services/pulse/manufacturing/batches/stages/delays` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Stage](stage.md) 
- [Delay](../master-data/delay.md) 
 
Create or retrieve both records before submitting the stage delay.