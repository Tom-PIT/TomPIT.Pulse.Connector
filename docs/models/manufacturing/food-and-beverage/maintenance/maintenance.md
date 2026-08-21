# Maintenance

Represents a preventive or corrective maintenance activity tracked in Pulse.

The Maintenance record identifies and classifies the activity. Related plan and usage records describe its planned and actual timing, resources, and costs.

## The Maintenance object

```json
{
  "id": 314,
  "code": "MNT-2026-0042",
  "kind": "Corrective",
  "reason": 17,
  "equipment": 82
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `314` |
| `code` | string | Unique business code used to identify the maintenance activity in the source system and Pulse. | `"MNT-2026-0042"` |
| `kind` | enum | Fundamental nature of the maintenance activity: `Preventive` or `Corrective`. | `"Corrective"` |
| [`reason`](../master-data/maintenance-reason.md) | integer | Optional Pulse `id` of the reason for the maintenance activity. | `17` |
| [`machine`](../master-data/machine.md) | integer | Optional Pulse `id` of the equipment associated with the maintenance activity. | `82` |

</div>

## Maintenance kind

`kind` distinguishes preventive maintenance from corrective maintenance.

| Value | Meaning |
| --- | --- |
| `Preventive` | A maintenance activity performed to prevent a failure, deterioration, or later downtime. |
| `Corrective` | A maintenance activity performed in response to an existing problem, failure, downtime, deviation, or deterioration. |

Preventive maintenance is performed before a failure to reduce operational risk. Corrective maintenance responds to an existing problem, failure, deviation, or deterioration.

> [!NOTE]
> The API model also includes `NotSet`. Use `Preventive` or `Corrective` when the maintenance kind is known.

## Related maintenance records

A maintenance record provides the identity and classification of the activity. Related records describe what was planned and what actually occurred.

- [Maintenance plan](maintenance-plan.md) records planned timing and planned resource requirements.
- [Maintenance usage](maintenance-usage.md) records actual timing and actual resource usage.
- Planned and actual resource records can reference energy sources, equipment, expenses, labor, and materials.
- [Downtime maintenance](../manufacturing/downtime-maintenance.md) connects a maintenance activity to a downtime record.

## Maintenance reason

A [maintenance reason](../master-data/maintenance-reason.md) describes the specific reason for a maintenance activity.

Maintenance kind and maintenance reason answer different questions:

- `kind` indicates whether the activity is preventive or corrective.
- Maintenance reason explains why the activity was required or performed.

For example, a maintenance activity may be classified as `Corrective`, while its maintenance reason identifies a bearing failure or another specific cause.

## API resource

| Service | Base path |
| --- | --- |
| `MaintenanceService` | `/services/pulse/maintenance` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

May also reference:

- [Maintenance reason](../master-data/maintenance-reason.md)
- [Machine](../master-data/machine.md)

Create or retrieve the applicable records before submitting the Maintenance record.

## Referenced by

- [Maintenance plans](maintenance-plan.md)
- [Maintenance usage records](maintenance-usage.md)
- [Downtime maintenance records](../manufacturing/downtime-maintenance.md)
- [Ambient value records](../manufacturing/ambient-value.md) that use the Maintenance [dimension](../data-model/dimension.md)