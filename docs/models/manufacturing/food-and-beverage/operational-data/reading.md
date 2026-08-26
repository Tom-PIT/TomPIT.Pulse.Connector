# Reading

Records a measured value captured at a specific point in time.

Readings can describe production resources and activities such as lines, machines, vessels, Runs, Batches, and Lots, including individual pack weights and other high-frequency measurements.

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
| `subject` | string | Business code of the resource or production activity being measured. | `"EQ003"` |
| [`measure`](../definitions-and-rules/measurement.md) | string | Business code of the Measurement being captured. | `"product-temp-holding"` |
| `value` | number, string, or boolean | Measured value. Its interpretation is determined by the Measurement definition. | `74.2` |
| `sensor` | string or null | Optional business code of the Machine, sensor, probe, or measurement point that produced the Reading. | `"EQ010-H06"` |
| `at` | string | Date and time when the value was captured, in ISO 8601 format. | `"2026-08-10T22:11:00+02:00"` |

</div>

> [!IMPORTANT]
> `subject`, `measure`, `sensor`, and `at` together identify a Reading.
>
> `subject` must identify an existing resource or production activity.
>
> `measure` must reference an existing Measurement.
>
> When `sensor` is provided, it must reference an existing registered measurement source.

## Choosing the subject

`subject` identifies the object or production activity that the measured value describes.

Typical subjects include:

- Production line
- Machine
- Vessel
- Run
- Batch
- Lot

For example:

```json
{
  "subject": "EQ003",
  "measure": "product-temp-holding",
  "value": 74.2,
  "sensor": "EQ003-TT-HOLD",
  "at": "2026-08-10T22:11:00+02:00"
}
```

A Run-level reading could instead use:

```json
{
  "subject": "L01-260810-002",
  "measure": "fill-weight",
  "value": 128.4,
  "sensor": "EQ010-H06",
  "at": "2026-08-10T22:11:02+02:00"
}
```

Attach the Reading to the subject the value actually describes.

For example, a Vessel temperature should use the Vessel as its subject rather than the Production line it supplies.

> [!IMPORTANT]
> A subject code must resolve unambiguously. The same code must not identify both a resource and a production activity.

## Sensors

Use `sensor` when the source of the measurement needs to be preserved or when several measurement points can report the same Measurement for the same subject.

For example:

```json
{
  "subject": "EQ003",
  "measure": "product-temp",
  "value": 74.2,
  "sensor": "EQ003-TT-01",
  "at": "2026-08-10T22:11:00+02:00"
}
```

and:

```json
{
  "subject": "EQ003",
  "measure": "product-temp",
  "value": 74.4,
  "sensor": "EQ003-TT-02",
  "at": "2026-08-10T22:11:00+02:00"
}
```

represent two different Readings because `sensor` is part of the composite key.

If no sensor needs to be distinguished, `sensor` can be omitted.

### Individual pack weights

For individual pack weights, `sensor` can identify the filling head or measurement point that produced each Reading:

```json
{
  "subject": "L01-260810-002",
  "measure": "fill-weight",
  "value": 128.4,
  "sensor": "EQ010-H06",
  "at": "2026-08-10T22:11:02+02:00"
}
```

Keeping the individual values and their sensor codes preserves the variation needed to compare filling heads and detect systematic overfill, underfill, or instability.

## Value type

The Measurement definition determines how `value` is interpreted.

Examples include:

```json
{
  "value": 74.2
}
```

for a numeric Measurement,

```json
{
  "value": true
}
```

for a Boolean Measurement, or:

```json
{
  "value": "high"
}
```

for a categorical Measurement.

Values must be compatible with the Measurement's declared value type.

## Plausibility

Numeric Measurements can define plausible bounds through `minValue` and `maxValue`.

When a Reading falls outside those bounds, Pulse retains the submitted value but marks it as bad-quality data internally.

This allows suspicious measurements to remain traceable rather than being silently discarded.

## Measurements and settings

Readings contain values that were actually measured.

Commanded or configured values are submitted separately through [Setting](setting.md).

```text
Measured value  → Reading
Commanded value → Setting
```

## Reference protection

A resource, production activity, or registered sensor referenced by an existing Reading cannot be deleted while the Reading still references it.

## API resource

| Resource | Base path |
| --- | --- |
| `Reading` | `/services/pulse/food-beverage/readings` |

## API methods

### Submit a reading

`POST /services/pulse/food-beverage/readings/insert`

Records one measured value.

#### Request

```http
POST /services/pulse/food-beverage/readings/insert
Content-Type: application/json
```

```json
{
  "subject": "EQ003",
  "measure": "product-temp-holding",
  "value": 74.2,
  "sensor": "EQ003-TT-HOLD",
  "at": "2026-08-10T22:11:00+02:00"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `subject` | string | yes | Business code of the measured resource or production activity. |
| `measure` | string | yes | Business code of the Measurement. |
| `value` | number, string, or boolean | yes | Captured value. |
| `sensor` | string or null | no | Registered measurement-source code. |
| `at` | string | yes | Date and time when the value was captured. |

### Update a reading

`PUT /services/pulse/food-beverage/readings/update`

Updates the value of an existing Reading.

The Reading is identified by `subject`, `measure`, `sensor`, and `at`.

#### Request

```http
PUT /services/pulse/food-beverage/readings/update
Content-Type: application/json
```

```json
{
  "subject": "EQ003",
  "measure": "product-temp-holding",
  "value": 74.6,
  "sensor": "EQ003-TT-HOLD",
  "at": "2026-08-10T22:11:00+02:00"
}
```

### Patch a reading

`PATCH /services/pulse/food-beverage/readings/patch`

Partially updates an existing Reading.

`properties.subject`, `properties.measure`, and `properties.at` are required to identify the Reading.

When the Reading has a `sensor`, include `properties.sensor` as part of the key.

PATCH changes the Reading's `value`; the composite key itself remains unchanged.

#### Request

```http
PATCH /services/pulse/food-beverage/readings/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "subject": "EQ003",
    "measure": "product-temp-holding",
    "sensor": "EQ003-TT-HOLD",
    "at": "2026-08-10T22:11:00+02:00",
    "value": 74.6
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.subject` | string | yes | Subject business code identifying the Reading. |
| `properties.measure` | string | yes | Measurement business code identifying the Reading. |
| `properties.sensor` | string or null | conditional | Sensor component of the Reading key. Include it when the Reading has a sensor. |
| `properties.at` | string | yes | Timestamp identifying the Reading. |
| `properties.value` | number, string, or boolean | no | New measured value. |

### Retrieve a reading

`GET /services/pulse/food-beverage/readings/select`

Returns the Reading identified by its composite business key.

#### Request

```http
GET /services/pulse/food-beverage/readings/select?subject=EQ003&measure=product-temp-holding&sensor=EQ003-TT-HOLD&at=2026-08-10T22:11:00%2B02:00
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `subject` | string | yes | Business code of the measured subject. |
| `measure` | string | yes | Business code of the Measurement. |
| `sensor` | string | no | Sensor component of the Reading key. |
| `at` | string | yes | Date and time of the Reading. |

#### Example response

```json
{
  "subject": "EQ003",
  "measure": "product-temp-holding",
  "value": 74.2,
  "sensor": "EQ003-TT-HOLD",
  "at": "2026-08-10T22:11:00+02:00"
}
```

### List readings

`GET /services/pulse/food-beverage/readings/query`

Returns Readings matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/readings/query?subjects=EQ003&measures=product-temp-holding
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `subjects` | string or array of strings | no | Limits results to the specified subject business codes. |
| `measures` | string or array of strings | no | Limits results to the specified Measurements. |
| `sensors` | string or array of strings | no | Limits results to the specified sensor codes. |
| `from` | string | no | Limits results to Readings captured at or after the specified date and time. |
| `to` | string | no | Limits results to Readings captured at or before the specified date and time. |

Multiple values can be supplied by repeating the query parameter:

```http
GET /services/pulse/food-beverage/readings/query?sensors=EQ003-TT-01&sensors=EQ003-TT-02
```

### Delete a reading

`DELETE /services/pulse/food-beverage/readings/delete`

Deletes the Reading identified by its composite business key.

#### Request

```http
DELETE /services/pulse/food-beverage/readings/delete?subject=EQ003&measure=product-temp-holding&sensor=EQ003-TT-HOLD&at=2026-08-10T22:11:00%2B02:00
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `subject` | string | yes | Business code of the measured subject. |
| `measure` | string | yes | Business code of the Measurement. |
| `sensor` | string | no | Sensor component of the Reading key. |
| `at` | string | yes | Date and time of the Reading. |