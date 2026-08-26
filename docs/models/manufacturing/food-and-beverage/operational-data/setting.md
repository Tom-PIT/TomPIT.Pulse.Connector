# Setting

<!-- TODO: This page is currently based on ApiSurfaceRevised. Revisit it once the implementation is available and verify fields, routes, batching behavior, correction rules, and examples against the current code. -->

Records a commanded or configured value applied to a Machine.

Settings are used for values such as target fill weight, speed setpoints, temperature setpoints, or other values that somebody explicitly sets on equipment.

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
| [`machine`](../master-data/machine.md) | string | Business code of the machine whose setting changed. | `"EQ010"` |
| [`measure`](../definitions-and-rules/measurement.md) | string | Business code of a Measurement declared with `parameterClass: "Setpoint"`. | `"fill-target"` |
| `value` | number, string, or boolean | Configured value. Its type must match the Measurement definition. | `130` |
| `at` | string | Date and time when the setting changed, in ISO 8601 format. | `"2026-08-10T22:10:00+02:00"` |

</div>

> [!IMPORTANT]
> `machine`, `measure`, and `at` together identify a setting record.
>
> `measure` must reference a Measurement declared as a `Setpoint`. Measurements declared as `Measurement` are submitted through [Readings](reading.md).

## Record changes

Submit a Setting when the configured value changes.

For example:

```json
[
  {
    "machine": "EQ010",
    "measure": "fill-target",
    "value": 130,
    "at": "2026-08-10T22:10:00+02:00"
  },
  {
    "machine": "EQ010",
    "measure": "fill-target",
    "value": 128,
    "at": "2026-08-11T02:15:00+02:00"
  }
]
```

The value remains in effect until another setting for the same machine and measurement is submitted.

There is no need to repeatedly submit the same value on a timer.

## Settings and readings

Settings describe what the machine was commanded to do.

[Readings](reading.md) describe what was actually measured.

```text
Commanded value → Settings
Measured value  → Readings
```

Keeping them separate allows Pulse to compare the configured value with the observed result.

For example:

```text
fill-target setting  → 128 g
fill-weight reading  → 131.2 g
```

The difference between those values can provide context for process performance and variation.

## Batching

The specification shows Settings accepting multiple records in one request:

```json
[
  {
    "machine": "EQ010",
    "measure": "fill-target",
    "value": 130,
    "at": "2026-08-10T22:10:00+02:00"
  },
  {
    "machine": "EQ010",
    "measure": "fill-target",
    "value": 128,
    "at": "2026-08-11T02:15:00+02:00"
  }
]
```

The exact batching behavior should be verified once the implementation is available.

## API resource

| Resource | Base path |
| --- | --- |
| `Setting` | `/services/pulse/food-beverage/settings` |

## API methods

> [!NOTE]
> The API methods below are provisional until the Settings implementation is available for verification.

### Submit settings

`POST /services/pulse/food-beverage/settings/insert`

Records commanded or configured machine values.

#### Request

```http
POST /services/pulse/food-beverage/settings/insert
Content-Type: application/json
```

```json
[
  {
    "machine": "EQ010",
    "measure": "fill-target",
    "value": 130,
    "at": "2026-08-10T22:10:00+02:00"
  },
  {
    "machine": "EQ010",
    "measure": "fill-target",
    "value": 128,
    "at": "2026-08-11T02:15:00+02:00"
  }
]
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `machine` | string | yes | Business code of the machine. |
| `measure` | string | yes | Business code of a Measurement declared as a `Setpoint`. |
| `value` | number, string, or boolean | yes | Configured value. |
| `at` | string | yes | Date and time when the setting changed. |