# Downtime

Represents a downtime event associated with a stage.

A downtime record identifies the operational context of the event. Planned and actual timing are submitted through the related downtime plan and downtime usage records.

## The Downtime object

```json
{
  "id": 630,
  "stage": 208,
  "equipment": 31,
  "type": 9,
  "cause": 11
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `630` |
| [`stage`](stage.md) | integer | Pulse `id` of the stage associated with the downtime. | `208` |
| [`machine`](../master-data/machine.md) | integer or null | Pulse `id` of the equipment associated with the downtime, when applicable. | `31` |
| [`type`](../master-data/downtime-type.md) | integer or null | Pulse `id` of the downtime type, when applicable. | `9` |
| [`cause`](../master-data/downtime-cause.md) | integer or null | Pulse `id` of the downtime cause, when applicable. | `11` |

</div>

## API resource

| Service | Base path |
| --- | --- |
| `DowntimeService` | `/services/pulse/manufacturing/batches/stages/downtime` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Stage](stage.md)

May also reference:

- [Machine](../master-data/machine.md)
- [Downtime type](../master-data/downtime-type.md)
- [Downtime cause](../master-data/downtime-cause.md)

Create or retrieve the applicable records before submitting the downtime record.

## Referenced by

- [Downtime plans](downtime-plan.md)
- [Downtime usage records](downtime-usage.md)
- [Downtime maintenance records](downtime-maintenance.md)