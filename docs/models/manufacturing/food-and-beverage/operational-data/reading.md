# Reading

<!-- TODO: This page is currently based on ApiSurfaceRevised. Revisit it once the implementation is available and verify fields, routes, batching behavior, correction rules, and examples against the current code. -->

Records a measured value captured at a specific point in time.

Readings can describe lines, machines, vessels, runs, batches, or lots, including individual pack weights and other high-frequency measurements.

## The Reading object

```json
{
  "subject": "EQ003",
  "measure": "product-temp-holding",
  "value": 74.2,
  "sensor": "EQ003-TT-HOLD",
  "at": "2026-08-10T22:11:00+02:00"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `subject` | string | Business code of the line, machine, vessel, run, batch, or lot being measured. | `"EQ003"` |
| [`measure`](../definitions-and-rules/measurements.md) | string | Business code of the Measurement being captured. | `"product-temp-holding"` |
| `value` | number, string, or boolean | Measured value. Its type must match the Measurement definition. | `74.2` |
| [`sensor`](../master-data/machine.md) | string or null | Optional business code of the sensor, probe, or filling head that produced the reading. | `"EQ003-TT-HOLD"` |
| `at` | string | Date and time when the value was captured, in ISO 8601 format. | `"2026-08-10T22:11:00+02:00"` |

</div>

> [!IMPORTANT]
> `measure`, `subject`, `sensor`, and `at` together identify a reading.
>
> `sensor` is part of the key when provided and must not be omitted when several sensors can report the same measurement for the same subject and timestamp.

## Choosing the subject

`subject` identifies exactly one object or activity that the reading describes.

Supported subjects include:

- Production line
- Machine
- Vessel
- Run
- Batch
- Lot

For example:

```json
[
  {
    "subject": "EQ003",
    "measure": "product-temp-holding",
    "value": 74.2,
    "sensor": "EQ003-TT-HOLD",
    "at": "2026-08-10T22:11:00+02:00"
  },
  {
    "subject": "L01-260810-002",
    "measure": "fill-weight",
    "value": 128.4,
    "sensor": "EQ010-H06",
    "at": "2026-08-10T22:11:02+02:00"
  }
]
```

Attach the reading to the subject the value actually describes.

For example, a vessel temperature should use the Vessel as its subject rather than the production line it supplies.

## Sensors

Use `sensor` when several measurement points can report the same measurement on the same subject.

For example:

```json
[
  {
    "subject": "EQ003",
    "measure": "product-temp",
    "value": 74.2,
    "sensor": "EQ003-TT-01",
    "at": "2026-08-10T22:11:00+02:00"
  },
  {
    "subject": "EQ003",
    "measure": "product-temp",
    "value": 74.4,
    "sensor": "EQ003-TT-02",
    "at": "2026-08-10T22:11:00+02:00"
  }
]
```

Without the sensor code, these readings would otherwise have the same key.

Sensors and filling heads are represented through the [Machine](../master-data/machine.md) resource.

## Value type

The type of `value` must match the value type declared by the Measurement.

Examples include:

```json
{
  "value": 74.2
}
```

for a continuous measurement,

```json
{
  "value": true
}
```

for a Boolean measurement, or:

```json
{
  "value": "high"
}
```

for a categorical measurement.

A value that does not match the declared Measurement type is rejected.

## Individual pack weights

Individual pack weights are submitted as Readings.

For example:

```json
{
  "subject": "L01-260810-002",
  "measure": "fill-weight",
  "value": 128.4,
  "sensor": "EQ010-H06",
  "at": "2026-08-10T22:11:02+02:00"
}
```

The `sensor` identifies the filling head that produced the pack.

Submit individual weighments rather than an average.

Individual values preserve the variation needed to compare filling heads and identify systematic overfill, underfill, or instability.

## Plausibility

Pulse can use the plausible bounds declared on the [Measurement](../definitions-and-rules/measurements.md) to identify readings that fall outside the expected sensor range.

Such values can be retained as suspect data rather than silently discarded, allowing them to be excluded deliberately during analysis.

## Measurements and settings

Readings contain values that were actually measured.

Commanded or configured values are submitted separately through [Settings](settings.md).

```text
Measured value  → Readings
Commanded value → Settings
```

The Measurement definition determines whether a code represents a Measurement or a Setpoint.

## Batching

The specification shows Readings accepting multiple records in one request:

```json
[
  {
    "subject": "EQ003",
    "measure": "product-temp-holding",
    "value": 74.2,
    "sensor": "EQ003-TT-HOLD",
    "at": "2026-08-10T22:11:00+02:00"
  },
  {
    "subject": "L01-260810-002",
    "measure": "fill-weight",
    "value": 128.4,
    "sensor": "EQ010-H06",
    "at": "2026-08-10T22:11:02+02:00"
  }
]
```

The exact batching behavior should be verified once the implementation is available.

## API resource

| Resource | Base path |
| --- | --- |
| `Reading` | `/services/pulse/food-beverage/readings` |

## API methods

> [!NOTE]
> The API methods below are provisional until the Readings implementation is available for verification.

### Submit readings

`POST /services/pulse/food-beverage/readings/insert`

Records measured values.

#### Request

```http
POST /services/pulse/food-beverage/readings/insert
Content-Type: application/json
```

```json
[
  {
    "subject": "EQ003",
    "measure": "product-temp-holding",
    "value": 74.2,
    "sensor": "EQ003-TT-HOLD",
    "at": "2026-08-10T22:11:00+02:00"
  },
  {
    "subject": "L01-260810-002",
    "measure": "fill-weight",
    "value": 128.4,
    "sensor": "EQ010-H06",
    "at": "2026-08-10T22:11:02+02:00"
  }
]
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `subject` | string | yes | Business code of the measured line, machine, vessel, run, batch, or lot. |
| `measure` | string | yes | Business code of the Measurement. |
| `value` | number, string, or boolean | yes | Captured value. |
| `sensor` | string or null | no | Business code of the sensor or measurement point. |
| `at` | string | yes | Date and time when the value was captured. |