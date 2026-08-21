# Labor usage period

Represents a specific interval during which labor was actually performed. A [labor usage](labor-usage.md) record may contain multiple periods when work occurred during separate intervals.

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

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier of the actual labor interval. | `944` |
| [`usage`](labor-usage.md) | integer | Pulse `id` of the labor usage record to which the interval belongs. | `692` |
| `start` | string | Actual start timestamp of the work, in ISO 8601 format. | `"2026-07-20T08:42:00+02:00"` |
| `end` | string | Actual end timestamp of the work, in ISO 8601 format. | `"2026-07-20T10:35:00+02:00"` |

</div>

## API resource

| Service | Base path |
| --- | --- |
| `LaborUsagePeriodService` | `/services/pulse/manufacturing/batches/stages/usage/labor/periods` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Labor usage](labor-usage.md)

Create or retrieve the labor usage record before submitting the labor usage period.