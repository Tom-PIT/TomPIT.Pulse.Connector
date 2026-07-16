# Material plan

Represents the planned use of material during a stage.

A stage can require several materials, and the same material can be planned for use across different stages and batches. Material plans allow Pulse to understand how much material is expected to be consumed and the planned cost of that consumption.

Pulse can compare this planned consumption with the related [material usage](material-usage.md) to identify differences in quantity and cost.

## The Material plan object

```json
{
  "id": 487,
  "stage": 208,
  "material": 63,
  "quantity": 125.0,
  "price": 4.2
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier of the planned material consumption record. | `487` |
| [`stage`](stage.md) | integer | Pulse `id` of the stage for which the material use is planned. | `208` |
| [`material`](../master-data/material.md) | integer | Pulse `id` of the material planned for use. | `63` |
| `quantity` | number | Planned quantity of the material. | `125.0` |
| `price` | number or null | Optional planned price per measure unit. | `4.2` |

</div>

The measure unit is defined by the related [material](../master-data/material.md).

## API service

| Service | Base path |
| --- | --- |
| `MaterialPlanService` | `/services/pulse/manufacturing/batches/stages/plan/materials` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Stage](stage.md)
- [Material](../master-data/material.md)

Create or retrieve the applicable records before submitting the material plan.

## Referenced by

- [Material usage records](material-usage.md)