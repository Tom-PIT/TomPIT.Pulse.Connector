# Dimension

A dimension identifies the operational context associated with a record.

The `dimension` value defines the type of context, while `dimensionId` identifies the specific Pulse record within that context.

For example:

```json
{
  "dimension": 3,
  "dimensionId": 208
}
```

This combination identifies Stage `208`.

## Supported dimensions

| Value | Dimension | `dimensionId` identifies |
| --- | --- | --- |
| `0` | Other | Another supported context |
| `1` | Plant | A [plant](../master-data/plant.md) |
| `2` | Production line | A [production line](../master-data/production-line.md) |
| `3` | Stage | A [stage](../manufacturing/stage.md) |
| `4` | Shift | A [shift](../master-data/shift.md) |
| `5` | Product | A [product](../master-data/product.md) |
| `6` | Batch | A [batch](../manufacturing/batch.md) |
| `7` | Energy source | An [energy source](../master-data/energy-source.md) |
| `8` | Equipment | An [equipment](../master-data/equipment.md) record |
| `9` | Expense | An [expense](../master-data/expense.md) |
| `10` | Labor | A [labor](../master-data/labor.md) record |
| `11` | Material | A [material](../master-data/material.md) |
| `12` | Downtime category | A [downtime category](../master-data/downtime-category.md) |
| `13` | Downtime type | A [downtime type](../master-data/downtime-type.md) |
| `14` | Downtime cause | A [downtime cause](../master-data/downtime-cause.md) |
| `15` | Waste type | A [waste type](../master-data/waste-type.md) |
| `16` | Delay | A [delay](../master-data/delay.md) |
| `17` | Maintenance | A [maintenance record](../maintenance/maintenance.md) |
| `18` | Maintenance kind | A preventive or corrective [maintenance classification](../maintenance/maintenance.md#maintenance-kind) |

## Selecting a dimension

Use the dimension that corresponds to the context represented by the source record. The selected dimension determines how Pulse interprets `dimensionId`.

> [!IMPORTANT]
> More precise context enables more precise analysis. A value linked only to a plant can describe broader environmental conditions, while a value linked to a production line, stage, equipment record, or batch allows Pulse to evaluate the relationship between those conditions and a specific operational result more accurately.

## Used by

- [Ambient value](../manufacturing/ambient-value.md)