# Stage usage

Represents the actual execution time of a stage.

## The Stage usage object

```json
{
  "id": 208,
  "start": "2026-07-20T08:07:00+02:00",
  "end": "2026-07-20T10:48:00+02:00"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| [`id`](stage.md) | integer | Unique identifier of the stage usage record. It is identical to the `id` of the related stage. | `208` |
| `start` | string or null | Optional actual stage start timestamp in ISO 8601 format. | `"2026-07-20T08:07:00+02:00"` |
| `end` | string or null | Optional actual stage end timestamp in ISO 8601 format. | `"2026-07-20T10:48:00+02:00"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `StageUsageService` | `/services/pulse/manufacturing/batches/stages/usage` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Stage](stage.md)

Create or retrieve the stage before submitting the usage record.