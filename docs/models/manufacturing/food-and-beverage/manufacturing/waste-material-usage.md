# Waste material usage

Represents material consumed, lost, or otherwise attributed to a waste record.

The quantity remains separate from the [waste](waste.md) record so Pulse can identify which material contributed to the loss and calculate its material cost.

## The Waste material usage object

```json
{
  "id": 813,
  "waste": 812,
  "quantity": 18.5,
  "material": 42,
  "price": 0.68,
  "supplier": 18,
  "lot": 92
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier of the waste material usage record. | `813` |
| [`waste`](waste.md) | integer | Pulse `id` of the waste record to which the material loss is attributed. | `812` |
| `quantity` | number | Quantity of material consumed or lost because of the waste. | `18.5` |
| [`material`](../master-data/material.md) | integer | Pulse `id` of the material consumed or lost. | `42` |
| `price` | number or null | Optional price per measure unit for this material loss. | `0.68` |
| [`supplier`](../master-data/supplier.md) | integer or null | Optional Pulse `id` of the supplier associated with the material. | `18` |
| [`lot`](../traceability/lot.md) | integer or null | Optional Pulse `id` of the lot associated with the material. | `92` |

</div>

The measure unit is defined by the related [material](../master-data/material.md).

## API resource

| Service | Base path |
| --- | --- |
| `WasteMaterialUsageService` | `/services/pulse/manufacturing/batches/stages/usage/waste/materials` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Waste](waste.md)
- [Material](../master-data/material.md)

May also reference:

- [Supplier](../master-data/supplier.md)
- [Lot](../traceability/lot.md)

Create or retrieve the applicable records before submitting the waste material usage record.
