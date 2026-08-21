# Ambient value

Represents a measured or received value at a specific time and within a specific operational context.

An ambient value is not limited to environmental measurements. It can describe product temperature, tank temperature, humidity, pressure, pH, or another measurable condition that may help explain an operational result.

The related [ambient type](../master-data/ambient-type.md) defines what is being measured and, when applicable, its measure unit and expected range. The ambient value records the actual observation and connects it to a specific Pulse context through `dimension` and `dimensionId`.

> [!NOTE]
> Pulse may store Ambient value records across multiple shards to support high data volumes. Sharding is handled internally and does not change how integrations submit or retrieve Ambient value records. See [Ambient value sharding](../data-model/ambient-value-sharding.md).

## The Ambient value object

```json
{
  "id": 7421,
  "date": "2026-07-20T09:18:00+02:00",
  "dimension": 3,
  "dimensionId": 208,
  "type": 22,
  "min": "72",
  "max": "75",
  "expected": "73",
  "value": "72.4"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `7421` |
| `date` | string | Date and time to which the measured value applies, in ISO 8601 format. | `"2026-07-20T09:18:00+02:00"` |
| [`dimension`](../data-model/dimension.md) | enum | Context in which the value was measured. Determines how `dimensionId` is interpreted. | `3` |
| `dimensionId` | integer | Pulse `id` of the specific record identified by `dimension`. | `208` |
| [`type`](../master-data/ambient-type.md) | integer | Pulse `id` of the ambient type that defines what the value represents. | `22` |
| `min` | string or null | Optional minimum permitted value for this measurement and context. | `"72"` |
| `max` | string or null | Optional maximum permitted value for this measurement and context. | `"75"` |
| `expected` | string or null | Optional expected, target, or reference value for this measurement and context. | `"73"` |
| `value` | string or null | Optional measured or received value. | `"72.4"` |

</div>

## Dimension context

The combination of [`dimension`](../data-model/dimension.md) and `dimensionId` identifies what the measurement applies to.

For example:

```json
{
  "dimension": 3,
  "dimensionId": 208
}
```

means that the value applies to Pasteurization Stage `208`.

> [!NOTE]
> More precise context enables more precise analysis. A value linked only to a plant can describe broader environmental conditions, while a value linked to a production line, stage, equipment record, or batch allows Pulse to evaluate its relationship with a specific production result more accurately.

## Measurement values and limits

`value` contains the actual measured or received value.

`min`, `max`, and `expected` provide an interpretation range for the specific observation and context:

- `min` is the lower permitted limit.
- `max` is the upper permitted limit.
- `expected` is the expected, target, or reference value.

These fields are represented as strings. Submit values in a format consistent with the related ambient type and source system.

The limits defined on an ambient value can provide context-specific thresholds. The same ambient type may have different permitted or expected values for different plants, lines, stages, equipment, or other contexts.

## API resource

| Service | Base path |
| --- | --- |
| `AmbientValueService` | `/services/pulse/manufacturing/ambient-values` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Ambient type](../master-data/ambient-type.md)
- The record identified by `dimension` and `dimensionId`

Create or retrieve the applicable records before submitting the ambient value.