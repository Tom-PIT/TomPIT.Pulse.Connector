# Reading

Represents one measured or commanded value captured at a specific time.

Readings are used for process measurements, environmental measurements, individual weighments, machine values, setpoints, and other signals declared through [Metrics](../master-data/metric.md).

## The Reading object

```json
{
  "metric": "product-temp",
  "value": 74.2,
  "unit": "C",
  "machine": "PASTEURIZER-01",
  "sensor": "PASTEURIZER-01-TT-HOLD",
  "at": "2026-08-10T22:15:00+02:00"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| [`metric`](../master-data/metric.md) | string | Code of the metric being captured. | `"product-temp"` |
| `value` | number, string, or boolean | Value captured for the metric. Its type must match the metric declaration. | `74.2` |
| `unit` | string or null | Optional unit associated with the captured value. | `"C"` |
| [`run`](run.md) | string | Code of the production run to which the reading belongs. Exactly one reading subject must be provided. | `"L03-260810-002"` |
| [`batch`](batch.md) | string | Code of the process batch to which the reading belongs. Exactly one reading subject must be provided. | `"BULK-260810-07"` |
| [`line`](../master-data/production-line.md) | string | Code of the production line to which the reading belongs. Exactly one reading subject must be provided. | `"L03"` |
| [`machine`](../master-data/machine.md) | string | Code of the machine to which the reading belongs. Exactly one reading subject must be provided. | `"PASTEURIZER-01"` |
| [`vessel`](../master-data/vessel.md) | string | Code of the vessel to which the reading belongs. Exactly one reading subject must be provided. | `"TANK-03"` |
| [`lot`](../master-data/lot.md) | string | Code of the lot to which the reading belongs. Exactly one reading subject must be provided. | `"MILK-2026-0717-A"` |
| [`sensor`](../master-data/machine.md) | string or null | Optional code of the sensor or measurement point that produced the reading. Sensors are registered as child machines. | `"PASTEURIZER-01-TT-HOLD"` |
| `at` | string | Timestamp when the value was captured, in ISO 8601 format with an explicit offset. | `"2026-08-10T22:15:00+02:00"` |

</div>

Exactly one of `run`, `batch`, `line`, `machine`, `vessel`, or `lot` must identify the subject of the reading.

## Choosing the subject

Attach the reading to the entity or activity the value actually describes.

For example:

```json
[
  {
    "metric": "ph",
    "value": 4.31,
    "run": "L03-260810-002",
    "at": "2026-08-10T22:15:00+02:00"
  },
  {
    "metric": "line-speed",
    "value": 412,
    "line": "L03",
    "at": "2026-08-10T22:15:00+02:00"
  },
  {
    "metric": "product-temp",
    "value": 74.2,
    "machine": "PASTEURIZER-01",
    "at": "2026-08-10T22:15:00+02:00"
  }
]
```

A vessel reading should be attached to the vessel rather than to the line it supplies.

Likewise, a value that describes a production run should reference the run rather than a machine that happened to capture it.

A reading that names more than one subject is rejected as ambiguous.

## Sensors

Use `sensor` when several measurement points capture the same metric on the same subject.

For example, a tank may have separate temperature probes at the top and bottom:

```json
[
  {
    "metric": "tank-temp",
    "value": 5.1,
    "vessel": "TANK-03",
    "sensor": "TANK-03-TT-TOP",
    "at": "2026-08-10T22:15:00+02:00"
  },
  {
    "metric": "tank-temp",
    "value": 5.4,
    "vessel": "TANK-03",
    "sensor": "TANK-03-TT-BOTTOM",
    "at": "2026-08-10T22:15:00+02:00"
  }
]
```

Sensors are registered through the [Machine](../master-data/machine.md) resource as child machines.

Use separate sensors when the probes measure the same thing at different measurement points.

When the position changes the meaning of the measurement, use separate metric codes instead.

For example:

```text
product-temp-inlet
product-temp-holding
product-temp-outlet
```

should be separate metrics rather than three sensors reporting one `product-temp` metric.

This prevents measurements with different process meaning from being combined. :contentReference[oaicite:1]{index=1}

## Individual weighments

Individual product weighments are readings.

For example:

```json
{
  "metric": "fill-weight",
  "value": 128.4,
  "unit": "g",
  "run": "L01-260810-002",
  "sensor": "FILLER-01-H06",
  "at": "2026-08-10T22:11:04+02:00"
}
```

The `sensor` identifies the filling head that produced the item.

Submit individual weighments rather than an average or summary.

The distribution of individual weights is what allows Pulse to analyse giveaway, variability, and differences between filling heads. :contentReference[oaicite:2]{index=2}

## Setpoints

Setpoints use the same Reading resource as measured values.

The distinction between a measurement and a setpoint is defined by the [Metric](../master-data/metric.md), not by a separate endpoint.

For example, when `fill-target` is declared with:

```json
{
  "code": "fill-target",
  "class": "Setpoint"
}
```

a change in the commanded value can be submitted as:

```json
{
  "metric": "fill-target",
  "value": 506,
  "unit": "g",
  "machine": "FILLER-01",
  "at": "2026-08-10T22:10:00+02:00"
}
```

Setpoints should normally be submitted when the commanded value changes rather than repeatedly sending the same value. :contentReference[oaicite:3]{index=3}

## Capture individual readings

Submit individual captures rather than pre-calculated summaries.

For example, do not replace a series of product temperatures or fill weights with one average value.

Individual readings preserve the variation and timing needed to detect relationships that an average can hide.

This is especially important for measurements such as fill weight, where the distribution is analytically meaningful.

## Batching

The Reading resource accepts either one object or an array of objects.

For example:

```json
[
  {
    "metric": "product-temp",
    "value": 74.2,
    "machine": "PASTEURIZER-01",
    "at": "2026-08-10T22:15:00+02:00"
  },
  {
    "metric": "product-temp",
    "value": 74.4,
    "machine": "PASTEURIZER-01",
    "at": "2026-08-10T22:15:30+02:00"
  }
]
```

This allows live sources to submit readings individually and historians or other buffered integrations to submit several captures together.

## API resource

| Resource | Base path |
| --- | --- |
| Reading | `/services/pulse/food-beverage/readings` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Metric](../master-data/metric.md)
- Exactly one reading subject:
    - [Run](run.md)
    - [Batch](batch.md)
    - [Production line](../master-data/production-line.md)
    - [Machine](../master-data/machine.md)
    - [Vessel](../master-data/vessel.md)
    - [Lot](../master-data/lot.md)
- [Machine](../master-data/machine.md), when `sensor` is provided

The metric and referenced subject must be available before submitting the reading.

When `sensor` is provided, the referenced sensor must also be registered as a machine.