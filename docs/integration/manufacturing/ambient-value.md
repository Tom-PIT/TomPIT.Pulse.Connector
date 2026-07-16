# Ambient value

Represents a measured or received value at a specific time and within a specific operational context.

An ambient value is not limited to environmental measurements. It can describe temperature, humidity, pressure, vibration, dust, airflow, lighting, machine temperature, air quality, weather conditions, or another measurable condition that may help explain an operational result.

The related [ambient type](../master-data/ambient-type.md) defines what is being measured and, when applicable, its measure unit and expected range. The ambient value records the actual observation and connects it to a specific Pulse context through `dimension` and `dimensionId`.

> [!NOTE]
> Pulse may store Ambient value records across multiple shards to support high data volumes. Sharding is handled internally and does not change how integrations submit or retrieve Ambient value records. See [Ambient value sharding](../../data-model/ambient-value-sharding.md).

## The Ambient value object

```json
{
  "id": 7421,
  "date": "2026-07-20T09:18:00+02:00",
  "dimension": 3,
  "dimensionId": 208,
  "type": 22,
  "min": "18",
  "max": "26",
  "expected": "22",
  "value": "28.4"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `7421` |
| `date` | string | Date and time to which the measured value applies, in ISO 8601 format. | `"2026-07-20T09:18:00+02:00"` |
| `dimension` | enum | Context in which the value was measured. Determines how `dimensionId` is interpreted. | `3` |
| `dimensionId` | integer | Pulse `id` of the specific record identified by `dimension`. | `208` |
| [`type`](../master-data/ambient-type.md) | integer | Pulse `id` of the ambient type that defines what the value represents. | `22` |
| `min` | string or null | Optional minimum permitted value for this measurement and context. | `"18"` |
| `max` | string or null | Optional maximum permitted value for this measurement and context. | `"26"` |
| `expected` | string or null | Optional expected, target, or reference value for this measurement and context. | `"22"` |
| `value` | string or null | Optional measured or received value. | `"28.4"` |

</div>

## Dimension context

The combination of `dimension` and `dimensionId` identifies what the measurement applies to.

For example:

```json
{
  "dimension": 3,
  "dimensionId": 208
}
```

means that the value applies to Stage `208`.

Supported `dimension` values are:

| Value | Dimension | `dimensionId` identifies |
| --- | --- | --- |
| `0` | Other | Another supported context |
| `1` | Plant | A [plant](../master-data/plant.md) |
| `2` | Production line | A [production line](../master-data/production-line.md) |
| `3` | Stage | A [stage](stage.md) |
| `4` | Shift | A [shift](../master-data/shift.md) |
| `5` | Product | A [product](../master-data/product.md) |
| `6` | Batch | A [batch](batch.md) |
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

The selected dimension determines the operational context in which Pulse interprets the value. Use the dimension that corresponds to the context represented by the source record. A value linked to a stage, batch, production line, or equipment record can usually support more precise analysis than a value linked only to a plant.

## Measurement values and limits

`value` contains the actual measured or received value.

`min`, `max`, and `expected` provide an interpretation range for the specific observation and context:

- `min` is the lower permitted limit.
- `max` is the upper permitted limit.
- `expected` is the expected, target, or reference value.

These fields are represented as strings. Submit values in a format consistent with the related ambient type and source system.

The limits defined on an ambient value can provide context-specific thresholds. The same ambient type may have different permitted or expected values for different plants, lines, stages, equipment, or other contexts.

## API service

| Service | Base path |
| --- | --- |
| `AmbientValueService` | `/services/pulse/manufacturing/ambient-values` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Ambient type](../master-data/ambient-type.md)
- The record identified by `dimension` and `dimensionId`

Create or retrieve the applicable records before submitting the ambient value.