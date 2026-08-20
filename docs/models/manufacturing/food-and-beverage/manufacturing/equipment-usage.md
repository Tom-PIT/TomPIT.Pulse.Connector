# Equipment usage

Represents the actual use of equipment during a stage.

Equipment usage records identify which equipment was used, how much it was used, the actual price or cost basis when known, and when the usage was recorded.

Pulse can compare this actual use with the related [equipment plan](equipment-plan.md). When the exact timing of the use is important, add one or more [equipment usage periods](equipment-usage-period.md).

## The Equipment usage object

```json
{
  "id": 704,
  "stage": 208,
  "equipment": 34,
  "quantity": 3.0,
  "price": 52.0,
  "date": "2026-07-20T10:48:00+02:00"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier of the equipment usage record. | `704` |
| [`stage`](stage.md) | integer | Pulse `id` of the stage in which the equipment was used. | `208` |
| [`equipment`](../master-data/equipment.md) | integer | Pulse `id` of the equipment that was used. | `34` |
| `quantity` | number | Actual quantity of equipment use in hours. | `3.0` |
| `price` | number or null | Optional actual hourly price or cost basis for the equipment use. | `52.0` |
| `date` | string | Date and time when the equipment use was recorded or occurred, in ISO 8601 format. | `"2026-07-20T10:48:00+02:00"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `EquipmentUsageService` | `/services/pulse/manufacturing/batches/stages/usage/equipment` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Stage](stage.md)
- [Equipment](../master-data/equipment.md)

Create or retrieve the applicable records before submitting the equipment usage record.

## Referenced by

- [Equipment usage periods](equipment-usage-period.md)