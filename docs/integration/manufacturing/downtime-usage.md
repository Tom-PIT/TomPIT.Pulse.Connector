# Downtime usage

Represents the actual time interval of a downtime event.

A downtime usage record belongs to a [downtime](downtime.md) record. Its `id` is identical to the `id` of the related downtime.

## The Downtime usage object

```json
{
  "id": 146,
  "start": "2026-07-20T10:04:00+02:00",
  "end": "2026-07-20T10:46:00+02:00"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| [`id`](downtime.md) | integer | Unique identifier of the downtime usage record. It is identical to the `id` of the related downtime. | `146` |
| `start` | string or null | Optional actual downtime start timestamp in ISO 8601 format. | `"2026-07-20T10:04:00+02:00"` |
| `end` | string or null | Optional actual downtime end timestamp in ISO 8601 format. | `"2026-07-20T10:46:00+02:00"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `DowntimeUsageService` | `/services/pulse/manufacturing/batches/stages/downtime/usage` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Downtime](downtime.md)

Create or retrieve the downtime record before submitting the downtime usage record.