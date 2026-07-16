# Labor usage period

Represents a specific time interval during which labor was actually performed.

A labor usage record identifies the labor role or type of work and the actual quantity used. A labor usage period adds the exact timing of that work.

This allows Pulse to compare planned and actual work schedules and identify late work, longer-than-planned activity, waiting, handover issues, or labor associated with downtime, waste, and quality deviations.

## The Labor usage period object

```json
{
  "id": 944,
  "usage": 692,
  "start": "2026-07-20T08:42:00+02:00",
  "end": "2026-07-20T10:35:00+02:00"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier of the actual labor interval. | `944` |
| [`usage`](labor-usage.md) | integer | Pulse `id` of the labor usage record to which the interval belongs. | `692` |
| `start` | string | Actual start timestamp of the work, in ISO 8601 format. | `"2026-07-20T08:42:00+02:00"` |
| `end` | string | Actual end timestamp of the work, in ISO 8601 format. | `"2026-07-20T10:35:00+02:00"` |

</div>

A single [labor usage](labor-usage.md) record may contain multiple periods when the work occurred during separate intervals.

## API service

| Service | Base path |
| --- | --- |
| `LaborUsagePeriodService` | `/services/pulse/manufacturing/batches/stages/usage/labor/periods` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Labor usage](labor-usage.md)

Create or retrieve the labor usage record before submitting the labor usage period.