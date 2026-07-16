# Equipment plan period

Represents a specific time interval during which equipment is planned for use.

An equipment plan identifies which equipment is expected to be used during a stage. An equipment plan period adds the exact timing of that planned use.

This is useful when equipment is shared, capacity is limited, or the timing of access matters—for example, with bottleneck equipment, tools, laboratory equipment, packaging lines, or inspection stations.

## The Equipment plan period object

```json
{
  "id": 913,
  "plan": 512,
  "start": "2026-07-20T08:30:00+02:00",
  "end": "2026-07-20T10:00:00+02:00"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier of the planned equipment usage interval. | `913` |
| [`plan`](equipment-plan.md) | integer | Pulse `id` of the equipment plan to which the interval belongs. | `512` |
| `start` | string | Planned start timestamp of the equipment use, in ISO 8601 format. | `"2026-07-20T08:30:00+02:00"` |
| `end` | string | Planned end timestamp of the equipment use, in ISO 8601 format. | `"2026-07-20T10:00:00+02:00"` |

</div>

A single [equipment plan](equipment-plan.md) may contain multiple periods when the equipment is planned for use during separate intervals.

## API service

| Service | Base path |
| --- | --- |
| `EquipmentPlanPeriodService` | `/services/pulse/manufacturing/batches/stages/plan/equipment/periods` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Equipment plan](equipment-plan.md)

Create or retrieve the equipment plan before submitting the equipment plan period.