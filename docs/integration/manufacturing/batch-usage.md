# Batch usage

Represents the actual execution time of a batch.

## The Batch usage object

```json
{
  "id": 105,
  "start": "2026-07-20T06:12:00+02:00",
  "end": "2026-07-20T14:37:00+02:00"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| [`id`](batch.md) | integer | Unique identifier of the batch usage record. It is identical to the `id` of the related batch. | `105` |
| `start` | string or null | Actual start timestamp in ISO 8601 format. | `"2026-07-20T06:12:00+02:00"` |
| `end` | string or null | Actual end timestamp in ISO 8601 format. | `"2026-07-20T14:37:00+02:00"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `BatchUsageService` | `/services/pulse/manufacturing/batches/usage` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Batch](batch.md) 

Create or retrieve the batch before submitting the usage record.