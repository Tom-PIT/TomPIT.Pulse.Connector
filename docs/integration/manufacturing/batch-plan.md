# Batch plan

Represents the planned timing and quantity of a batch.

## The Batch plan object

```json
{
  "id": 105,
  "start": "2026-07-20T06:00:00+02:00",
  "end": "2026-07-20T14:00:00+02:00",
  "quantity": 500
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| [`id`](batch.md) | integer | Unique identifier of the batch plan. It is identical to the `id` of the related batch. | `105` |
| `start` | string or null | Optional planned start timestamp in ISO 8601 format. | `"2026-07-20T06:00:00+02:00"` |
| `end` | string or null | Optional planned end timestamp in ISO 8601 format. | `"2026-07-20T14:00:00+02:00"` |
| `quantity` | number | Planned batch quantity. | `500` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `BatchPlanService` | `/services/pulse/manufacturing/batches/plan` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Batch](batch.md)

Create or retrieve the batch before submitting the batch plan.