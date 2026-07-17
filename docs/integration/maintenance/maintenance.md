# Maintenance

Represents a maintenance activity tracked in Pulse.

A maintenance record identifies the activity and classifies its fundamental nature as preventive or corrective. Planned timing, actual timing, resource requirements, resource usage, and related downtime are recorded through the maintenance records that reference it.

## The Maintenance object

```json
{
  "id": 314,
  "code": "MNT-2026-0042",
  "kind": "Corrective"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `314` |
| `code` | string | Unique business code used to identify the maintenance activity in the source system and Pulse. | `"MNT-2026-0042"` |
| `kind` | enum | Fundamental nature of the maintenance activity: `Preventive` or `Corrective`. | `"Corrective"` |

</div>

## Maintenance kind

`kind` distinguishes preventive maintenance from corrective maintenance.

| Value | Meaning |
| --- | --- |
| `Preventive` | A maintenance activity performed to prevent a failure, deterioration, or later downtime. |
| `Corrective` | A maintenance activity performed in response to an existing problem, failure, downtime, deviation, or deterioration. |

This distinction is important because preventive and corrective maintenance have different operational and business meanings.

Preventive maintenance may introduce a planned cost and planned interruption, but it can reduce the risk of a larger loss later. Corrective maintenance usually indicates that a problem has already occurred and may already have affected time, capacity, quality, or cost.

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

## API service

| Service | Base path |
| --- | --- |
| `MaintenanceService` | `/services/pulse/maintenance` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Referenced by

- [Maintenance plans](maintenance-plan.md)
- [Maintenance usage records](maintenance-usage.md)
- [Downtime maintenance records](../manufacturing/downtime-maintenance.md)
- [Ambient value records](../manufacturing/ambient-value.md)