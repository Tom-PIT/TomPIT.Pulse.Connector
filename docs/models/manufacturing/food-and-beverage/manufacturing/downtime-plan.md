# Downtime plan

Represents the planned time interval of a downtime event.

A downtime plan belongs to a [downtime](downtime.md) record. Its `id` is identical to the `id` of the related downtime.

## The Downtime plan object

```json
{
  "id": 146,
  "start": "2026-07-20T10:00:00+02:00",
  "end": "2026-07-20T10:20:00+02:00"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| [`id`](downtime.md) | integer | Unique identifier of the downtime plan. It is identical to the `id` of the related downtime. | `146` |
| `start` | string or null | Optional planned downtime start timestamp in ISO 8601 format. | `"2026-07-20T10:00:00+02:00"` |
| `end` | string or null | Optional planned downtime end timestamp in ISO 8601 format. | `"2026-07-20T10:20:00+02:00"` |

</div>

## API resource

| Service | Base path |
| --- | --- |
| `DowntimePlanService` | `/services/pulse/manufacturing/batches/stages/downtime/plan` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Downtime](downtime.md)

Create or retrieve the downtime record before submitting the downtime plan.