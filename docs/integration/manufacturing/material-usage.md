# Material usage

Represents the actual use of material during a stage.

Material usage records identify which material was consumed, how much was used, the actual price when known, when the consumption was recorded or occurred, and the related lot when traceability information is available.

Pulse can compare this actual consumption with the related [material plan](material-plan.md) to identify differences in quantity and cost. The optional lot also allows Pulse to distinguish between separate traceable deliveries or batches of the same material.

## The Material usage object

```json
{
  "id": 733,
  "stage": 208,
  "material": 63,
  "quantity": 132.5,
  "price": 4.45,
  "date": "2026-07-20T10:48:00+02:00",
  "lot": 92
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier of the actual material consumption record. | `733` |
| [`stage`](stage.md) | integer | Pulse `id` of the stage in which the material was used. | `208` |
| [`material`](../master-data/material.md) | integer | Pulse `id` of the material that was used. | `63` |
| `quantity` | number | Actual quantity of the material used. | `132.5` |
| `price` | number or null | Optional actual price per measure unit for this consumption record. | `4.45` |
| `date` | string | Date and time when the material consumption was recorded or occurred, in ISO 8601 format. | `"2026-07-20T10:48:00+02:00"` |
| [`lot`](../../traceability/lot.md) | integer or null | Optional Pulse `id` of the lot associated with the consumed material. | `92` |

</div>

The measure unit is defined by the related [material](../master-data/material.md).

## API service

| Service | Base path |
| --- | --- |
| `MaterialUsageService` | `/services/pulse/manufacturing/batches/stages/usage/materials` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Stage](stage.md)
- [Material](../master-data/material.md)

May also reference:

- [Lot](../../traceability/lot.md)

Create or retrieve the applicable records before submitting the material usage record.