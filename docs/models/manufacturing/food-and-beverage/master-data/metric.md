# Metric

Represents a measurable or commanded signal used in Food & Beverage operations.

A metric defines what a value means, how it is captured, and how Pulse should interpret and aggregate it.

Metrics are declared before values are submitted through the readings API.

## The Metric object

```json
{
  "code": "cold-room-temp",
  "name": "Cold room temperature",
  "unit": "C",
  "valueType": "Continuous",
  "aggregation": "Avg",
  "semantic": "Ambient",
  "class": "Measurement",
  "capture": "Sampled",
  "declaredCadenceSeconds": 300,
  "plausibleMin": -40,
  "plausibleMax": 60
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the metric in source systems and integrations. | `"cold-room-temp"` |
| `name` | string | Human-readable name of the metric. | `"Cold room temperature"` |
| `unit` | string | Unit in which values for the metric are expressed. | `"C"` |
| `valueType` | string | Defines the kind of value carried by the metric. Supported values are `Continuous`, `Ordinal`, `Categorical`, and `Boolean`. | `"Continuous"` |
| `aggregation` | string | Defines how multiple values are aggregated. Supported values are `Sum`, `Avg`, `Min`, `Max`, `Last`, and `Mode`. | `"Avg"` |
| `semantic` | string | Describes the business meaning of the metric. | `"Ambient"` |
| `class` | string | Indicates whether the metric represents a measured value or a setpoint. Supported values are `Measurement` and `Setpoint`. | `"Measurement"` |
| `capture` | string | Defines how values are captured. Supported values are `Sampled` and `EventOnChange`. | `"Sampled"` |
| `declaredCadenceSeconds` | integer or null | Optional expected interval, in seconds, between sampled readings. | `300` |
| `plausibleMin` | number or null | Optional lower bound used to identify implausible measurements. | `-40` |
| `plausibleMax` | number or null | Optional upper bound used to identify implausible measurements. | `60` |

</div>

## Value types

Use `valueType` to describe the shape of values submitted for the metric.

| Value | Use for |
| --- | --- |
| `Continuous` | Numeric measurements such as temperature, pressure, weight, or speed |
| `Ordinal` | Ordered categories such as low, normal, and high |
| `Categorical` | Unordered categories such as operating mode or selector position |
| `Boolean` | Two-state values such as open/closed or on/off |

## Capture modes

Use `capture` to describe how the source system records the signal.

`Sampled` metrics are recorded repeatedly on a cadence, such as a temperature reading every five minutes.

`EventOnChange` metrics are recorded only when the value changes, such as a door opening or a setpoint being changed.

## Measurement and setpoint metrics

The `class` field distinguishes measured process values from commanded values.

For example:

```json
{
  "code": "product-temp",
  "class": "Measurement"
}
```

represents an observed temperature, while:

```json
{
  "code": "oven-setpoint",
  "class": "Setpoint"
}
```

represents a commanded target.

## Plausible bounds

`plausibleMin` and `plausibleMax` define physically plausible limits for a signal.

They are intended to identify invalid or broken measurements. They are not operational targets or specification limits.

Use [Expected values](../expected.md) to define the range in which a process or measurement is expected to operate.

## API resource

| Resource | Base path |
| --- | --- |
| Metric | `/services/pulse/food-beverage/metrics` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.