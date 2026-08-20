# Waste energy source usage

Represents energy consumption attributed to a waste record.

This record identifies which energy source was consumed and how much of that consumption is associated with the [waste](waste.md).

## The Waste energy source usage object

```json
{
  "id": 814,
  "waste": 812,
  "quantity": 42.0,
  "energySource": 17,
  "price": 0.21,
  "supplier": 18
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier of the waste energy source usage record. | `814` |
| [`waste`](waste.md) | integer | Pulse `id` of the waste record to which the energy consumption is attributed. | `812` |
| `quantity` | number | Quantity of the energy source attributed to the waste. | `42.0` |
| [`energySource`](../master-data/energy-source.md) | integer | Pulse `id` of the energy source that was consumed. | `17` |
| `price` | number or null | Optional price per measure unit for this energy consumption. | `0.21` |
| [`supplier`](../master-data/supplier.md) | integer or null | Optional Pulse `id` of the supplier or supply context associated with the energy source. | `18` |

</div>

The measure unit is defined by the related [energy source](../master-data/energy-source.md).

## API service

| Service | Base path |
| --- | --- |
| `WasteEnergySourceUsageService` | `/services/pulse/manufacturing/batches/stages/usage/waste/energy-sources` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Waste](waste.md)
- [Energy source](../master-data/energy-source.md)

May also reference:

- [Supplier](../master-data/supplier.md)

Create or retrieve the applicable records before submitting the waste energy source usage record.
