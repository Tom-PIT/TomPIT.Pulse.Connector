# Setting

Records a commanded or configured value applied to a [Machine](../master-data/machine.md).

Settings are used for values such as target fill weight, speed setpoints, temperature setpoints, or other values explicitly configured on equipment.

## The Setting object

```json
{
  "machine": "EQ010",
  "measure": "fill-target",
  "value": 130,
  "at": "2026-08-10T22:10:00+02:00"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| [`machine`](../master-data/machine.md) | string | Business code of the Machine to which the Setting applies. | `"EQ010"` |
| [`measure`](../definitions-and-rules/measurement.md) | string | Business code of a Measurement declared with `parameterClass: "Setpoint"`. | `"fill-target"` |
| `value` | number, string, or boolean | Configured value. Its interpretation is determined by the Measurement definition. | `130` |
| `at` | string | Date and time when the Setting was applied, in ISO 8601 format. | `"2026-08-10T22:10:00+02:00"` |

</div>

> [!IMPORTANT]
> `machine`, `measure`, and `at` together identify a Setting.
>
> `machine` must reference an existing Machine.
>
> `measure` must reference an existing Measurement declared with `parameterClass: "Setpoint"`.

## Record changes

Submit a Setting when the configured value changes.

For example, these are two separate Setting records:

```json
{
  "machine": "EQ010",
  "measure": "fill-target",
  "value": 130,
  "at": "2026-08-10T22:10:00+02:00"
}
```

and:

```json
{
  "machine": "EQ010",
  "measure": "fill-target",
  "value": 128,
  "at": "2026-08-11T02:15:00+02:00"
}
```

Each record preserves the configured value together with the time at which it was applied.

There is normally no need to repeatedly submit the same unchanged value on a timer.

## Value type

The Measurement definition determines how `value` is interpreted.

For example:

```json
{
  "value": 130
}
```

for a numeric setpoint,

```json
{
  "value": true
}
```

for a Boolean setpoint, or:

```json
{
  "value": "high"
}
```

for a categorical setpoint.

Values must be compatible with the Measurement's declared value type.

## Plausibility

Numeric Measurements can define plausible bounds through `minValue` and `maxValue`.

When a numeric Setting falls outside those bounds, Pulse retains the submitted value but marks it as bad-quality data internally.

This preserves the configured value while allowing suspicious values to be identified during analysis.

## Settings and readings

Settings describe what the Machine was commanded or configured to do.

[Readings](reading.md) describe what was actually measured.

```text
Commanded value → Setting
Measured value  → Reading
```

Keeping them separate allows Pulse to compare the configured value with the observed result.

For example:

```text
fill-target Setting  → 128 g
fill-weight Reading  → 131.2 g
```

The difference between the two provides context for process performance and variation.

## Corrections

A Setting is identified by:

```text
machine + measure + at
```

Use `update` or `patch` to correct the configured value while keeping the same composite key.

For example, to correct:

```text
EQ010 / fill-target / 2026-08-10T22:10:00+02:00
```

submit the same key with a new `value`.

## Reference protection

A Machine referenced by an existing Setting cannot be deleted while the Setting still references it.

## API resource

| Resource | Base path |
| --- | --- |
| `Setting` | `/services/pulse/food-beverage/settings` |

## API methods

### Submit a setting

`POST /services/pulse/food-beverage/settings/insert`

Records one commanded or configured Machine value.

#### Request

```http
POST /services/pulse/food-beverage/settings/insert
Content-Type: application/json
```

```json
{
  "machine": "EQ010",
  "measure": "fill-target",
  "value": 130,
  "at": "2026-08-10T22:10:00+02:00"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `machine` | string | yes | Business code of the Machine. |
| `measure` | string | yes | Business code of a Measurement declared as a `Setpoint`. |
| `value` | number, string, or boolean | yes | Configured value. |
| `at` | string | yes | Date and time when the Setting was applied. |

### Update a setting

`PUT /services/pulse/food-beverage/settings/update`

Updates the value of an existing Setting.

The Setting is identified by `machine`, `measure`, and `at`.

#### Request

```http
PUT /services/pulse/food-beverage/settings/update
Content-Type: application/json
```

```json
{
  "machine": "EQ010",
  "measure": "fill-target",
  "value": 128,
  "at": "2026-08-10T22:10:00+02:00"
}
```

### Patch a setting

`PATCH /services/pulse/food-beverage/settings/patch`

Partially updates an existing Setting.

The Setting is identified by `properties.machine`, `properties.measure`, and `properties.at`. All three are required.

PATCH changes the `value`; the composite key itself remains unchanged.

#### Request

```http
PATCH /services/pulse/food-beverage/settings/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "machine": "EQ010",
    "measure": "fill-target",
    "at": "2026-08-10T22:10:00+02:00",
    "value": 128
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.machine` | string | yes | Machine business code identifying the Setting. |
| `properties.measure` | string | yes | Measurement business code identifying the Setting. |
| `properties.at` | string | yes | Timestamp identifying the Setting. |
| `properties.value` | number, string, or boolean | no | New configured value. |

### Retrieve a setting

`GET /services/pulse/food-beverage/settings/select`

Returns the Setting identified by its composite business key.

#### Request

```http
GET /services/pulse/food-beverage/settings/select?machine=EQ010&measure=fill-target&at=2026-08-10T22:10:00%2B02:00
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `machine` | string | yes | Business code of the Machine. |
| `measure` | string | yes | Business code of the Measurement. |
| `at` | string | yes | Date and time of the Setting. |

#### Example response

```json
{
  "machine": "EQ010",
  "measure": "fill-target",
  "value": 130,
  "at": "2026-08-10T22:10:00+02:00"
}
```

### List settings

`GET /services/pulse/food-beverage/settings/query`

Returns Settings matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/settings/query?machines=EQ010&measures=fill-target
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `machines` | string or array of strings | no | Limits results to the specified Machines. |
| `measures` | string or array of strings | no | Limits results to the specified Setpoint Measurements. |
| `from` | string | no | Limits results to Settings applied at or after the specified date and time. |
| `to` | string | no | Limits results to Settings applied at or before the specified date and time. |

Multiple values can be supplied by repeating the query parameter:

```http
GET /services/pulse/food-beverage/settings/query?machines=EQ010&machines=EQ011
```

### Delete a setting

`DELETE /services/pulse/food-beverage/settings/delete`

Deletes the Setting identified by its composite business key.

#### Request

```http
DELETE /services/pulse/food-beverage/settings/delete?machine=EQ010&measure=fill-target&at=2026-08-10T22:10:00%2B02:00
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `machine` | string | yes | Business code of the Machine. |
| `measure` | string | yes | Business code of the Measurement. |
| `at` | string | yes | Date and time of the Setting. |