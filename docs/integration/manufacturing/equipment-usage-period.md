# Equipment usage period

Represents a specific time interval during which equipment was actually used.

An equipment usage record may contain multiple periods when the equipment was used during separate intervals.

## The Equipment usage period object

```json
{
  "id": 952,
  "usage": 704,
  "start": "2026-07-20T08:42:00+02:00",
  "end": "2026-07-20T10:35:00+02:00"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier of the actual equipment usage interval. | `952` |
| [`usage`](equipment-usage.md) | integer | Pulse `id` of the equipment usage record to which the interval belongs. | `704` |
| `start` | string | Actual start timestamp of the equipment use, in ISO 8601 format. | `"2026-07-20T08:42:00+02:00"` |
| `end` | string | Actual end timestamp of the equipment use, in ISO 8601 format. | `"2026-07-20T10:35:00+02:00"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `EquipmentUsagePeriodService` | `/services/pulse/manufacturing/batches/stages/usage/equipment/periods` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Equipment usage](equipment-usage.md)

Create or retrieve the equipment usage record before submitting the equipment usage period.