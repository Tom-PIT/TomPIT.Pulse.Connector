# Batch shift

Represents the assignment of a shift to a batch during a specific time interval.

A batch can span one or more shifts. Batch shift records identify which shift was responsible for the batch during each part of its execution.

This allows Pulse to compare performance, resource use, delays, downtime, output, and other results between shifts while keeping them connected to the same batch.

## The Batch shift object

```json
{
  "id": 301,
  "batch": 105,
  "shift": 8,
  "start": "2026-07-20T06:00:00+02:00",
  "end": "2026-07-20T14:00:00+02:00"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `301` |
| [`batch`](batch.md) | integer | Pulse `id` of the batch associated with the shift. | `105` |
| [`shift`](../master-data/shift.md) | integer | Pulse `id` of the assigned shift. | `8` |
| `start` | string or null | Optional start timestamp of the shift assignment in ISO 8601 format. | `"2026-07-20T06:00:00+02:00"` |
| `end` | string or null | Optional end timestamp of the shift assignment in ISO 8601 format. | `"2026-07-20T14:00:00+02:00"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `BatchShiftService` | `/services/pulse/manufacturing/batches/shifts` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Batch](batch.md)
- [Shift](../master-data/shift.md) 

Create or retrieve both records before submitting the batch shift.