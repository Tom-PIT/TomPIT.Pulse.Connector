# Shift

Represents a defined work period used to organize operational activity in Pulse.

## The Shift object

```json
{
  "id": 8,
  "code": "MORNING",
  "name": "Morning shift"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `8` |
| `code` | string | Business code used to identify the shift in external systems and integrations. | `"MORNING"` |
| `name` | string | Human-readable name of the shift. | `"Morning shift"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `ShiftService` | `/services/pulse/types/shifts` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Used by

Shifts are referenced by [Batch shift records](../manufacturing/batch-shift-record.md).