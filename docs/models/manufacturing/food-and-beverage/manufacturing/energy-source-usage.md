# Energy source usage

Represents the actual use of an energy source during a stage.

Energy source usage records identify which energy source was consumed, how much was used, the actual price when known, when the consumption was recorded or occurred, and the related lot when traceability information is available.

Pulse can compare this actual consumption with the related [energy source plan](energy-source-plan.md) to identify differences in quantity and cost.

## The Energy source usage object

```json
{
  "id": 684,
  "stage": 208,
  "energySource": 17,
  "quantity": 347.5,
  "price": 0.21,
  "date": "2026-07-20T10:48:00+02:00",
  "lot": 92
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier of the actual energy consumption record. | `684` |
| [`stage`](stage.md) | integer | Pulse `id` of the stage in which the energy source was used. | `208` |
| [`energySource`](../master-data/energy-source.md) | integer | Pulse `id` of the energy source that was used. | `17` |
| `quantity` | number | Actual quantity of the energy source used. | `347.5` |
| `price` | number or null | Optional actual price per measure unit for this consumption record. | `0.21` |
| `date` | string | Date and time when the consumption was recorded or occurred, in ISO 8601 format. | `"2026-07-20T10:48:00+02:00"` |
| [`lot`](../traceability/lot.md) | integer or null | Optional Pulse `id` of the lot associated with the consumed energy source. | `92` |

</div>

The measure unit is defined by the related [energy source](../master-data/energy-source.md).

## API service

| Service | Base path |
| --- | --- |
| `EnergySourceUsageService` | `/services/pulse/manufacturing/batches/stages/usage/energy-sources` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Stage](stage.md)
- [Energy source](../master-data/energy-source.md)

May also reference:

- [Lot](../traceability/lot.md)

Create or retrieve the applicable records before submitting the energy source usage record.