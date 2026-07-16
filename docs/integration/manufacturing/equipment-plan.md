# Equipment plan

Represents the planned use of equipment during a [stage](stage.md).

A stage can require several pieces of equipment, and the same equipment can be planned for use across different stages and batches. Equipment plans allow Pulse to understand which operational assets are expected to be involved, how much they are expected to be used, and the planned cost of that use.

When the exact timing of the planned use is important, add one or more [equipment plan periods](equipment-plan-period.md).

## The Equipment plan object

```json
{
  "id": 512,
  "stage": 208,
  "equipment": 34,
  "quantity": 2.5,
  "price": 48.0
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier of the equipment plan. | `512` |
| [`stage`](stage.md) | integer | Pulse `id` of the stage in which the equipment is planned for use. | `208` |
| [`equipment`](../master-data/equipment.md) | integer | Pulse `id` of the equipment planned for use. | `34` |
| `quantity` | number | Planned quantity of equipment use. | `2.5` |
| `price` | number or null | Optional planned price for the equipment use. | `48.0` |

</div>

## Planned usage periods

The equipment plan identifies the equipment and the expected quantity and cost of its use.

When the schedule within the stage also matters, use [equipment plan periods](equipment-plan-period.md) to record the exact planned start and end of each usage interval.

A single equipment plan may therefore have one or more planned usage periods.

## API service

| Service | Base path |
| --- | --- |
| `EquipmentPlanService` | `/services/pulse/manufacturing/batches/stages/plan/equipment` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Stage](stage.md)
- [Equipment](../master-data/equipment.md)

Create or retrieve the applicable records before submitting the equipment plan.

## Referenced by

- [Equipment plan periods](equipment-plan-period.md)