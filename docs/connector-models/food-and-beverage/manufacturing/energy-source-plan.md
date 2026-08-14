# Energy source plan

Represents the planned use of an energy source during a stage.

Energy source plans allow Pulse to compare expected energy consumption and cost with the energy that was actually used. Different stages may require different energy sources, quantities, and prices.

## The Energy source plan object

```json
{
  "id": 501,
  "stage": 208,
  "energySource": 17,
  "quantity": 320.0,
  "price": 0.18
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier of the planned energy consumption record. | `501` |
| [`stage`](stage.md) | integer | Pulse `id` of the stage for which the energy use is planned. | `208` |
| [`energySource`](../master-data/energy-source.md) | integer | Pulse `id` of the energy source planned for use. | `17` |
| `quantity` | number | Planned quantity of the energy source. | `320.0` |
| `price` | number or null | Optional planned price per measure unit. | `0.18` |

</div>

The measure unit is defined by the related [energy source](../master-data/energy-source.md).

## API service

| Service | Base path |
| --- | --- |
| `EnergySourcePlanService` | `/services/pulse/manufacturing/batches/stages/plan/energy-sources` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Stage](stage.md)
- [Energy source](../master-data/energy-source.md)

Create or retrieve the applicable records before submitting the energy source plan.