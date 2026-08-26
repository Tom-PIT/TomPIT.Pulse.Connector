# Target

Defines expected operating values for production resources such as production lines, machines, and vessels.

Targets apply from a specific point in time and can define a minimum, target, maximum, or any combination of those values.

## The Target object

```json
{
  "where": "LINE001",
  "measure": "nominal-rate",
  "min": null,
  "target": 420,
  "max": null,
  "unit": "pcs/min",
  "from": "2026-01-01T00:00:00+01:00",
  "setBy": "Line capability study"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `where` | string | Business code of the production resource to which the target applies, such as a production line, machine, or vessel. | `"LINE001"` |
| [`measure`](measurement.md) | string | Business code of the measurement to which the target applies. | `"nominal-rate"` |
| `min` | number or null | Optional minimum expected value. | `400` |
| `target` | number or null | Optional desired value. | `420` |
| `max` | number or null | Optional maximum expected value. | `440` |
| `unit` | string | Unit associated with the referenced measurement. | `"pcs/min"` |
| `from` | string | ISO 8601 timestamp from which the target applies. | `"2026-01-01T00:00:00+01:00"` |
| `setBy` | string or null | Optional source that established the target. | `"Line capability study"` |

</div>

> [!IMPORTANT]
> `where` must reference an existing production resource and `measure` must reference an existing measurement.
>
> `unit` should match the unit declared for the referenced measurement.

## Examples

A production-line target:

```json
{
  "where": "LINE001",
  "measure": "nominal-rate",
  "target": 420,
  "unit": "pcs/min",
  "from": "2026-01-01T00:00:00+01:00",
  "setBy": "Line capability study"
}
```

An expected operating range for a vessel:

```json
{
  "where": "TANK-3",
  "measure": "fermentation-temp",
  "min": 38,
  "target": 40,
  "max": 42,
  "unit": "C",
  "from": "2026-01-01T00:00:00+01:00",
  "setBy": "Process specification"
}
```

## Effective dates

A Target is identified by the combination of:

- `where`
- `measure`
- `from`

Submitting the same combination again replaces the values for that Target.

Submitting the same production resource and measurement with a different `from` timestamp creates another Target for that effective moment.

For example, if a line's nominal rate changes on 1 July, submit a new Target with:

```json
{
  "where": "LINE001",
  "measure": "nominal-rate",
  "target": 440,
  "unit": "pcs/min",
  "from": "2026-07-01T00:00:00+02:00",
  "setBy": "Line capability study"
}
```

## Targets are not alarms

Targets provide operating context for analysis.

A reading outside a target range does not by itself create an event or alarm. Targets allow Pulse to compare actual operation with the value or range that was expected.

## API resource

| Resource | Base path |
| --- | --- |
| `Target` | `/services/pulse/food-beverage/targets` |

## API methods

### Submit a target

`POST /services/pulse/food-beverage/targets/insert`

Creates a Target or replaces an existing Target with the same `where`, `measure`, and `from` values.

#### Request

```http
POST /services/pulse/food-beverage/targets/insert
Content-Type: application/json
```

```json
{
  "where": "LINE001",
  "measure": "nominal-rate",
  "target": 420,
  "unit": "pcs/min",
  "from": "2026-01-01T00:00:00+01:00",
  "setBy": "Line capability study"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `where` | string | yes | Business code of the production resource to which the target applies. |
| `measure` | string | yes | Business code of the measurement. |
| `min` | number or null | no | Minimum expected value. |
| `target` | number or null | no | Desired value. |
| `max` | number or null | no | Maximum expected value. |
| `unit` | string | yes | Unit associated with the referenced measurement. |
| `from` | string | yes | ISO 8601 timestamp from which the target applies. |
| `setBy` | string or null | no | Optional source that established the target. |

### Correct a target

Targets do not use a separate update endpoint.

To correct an existing Target, submit it again through the `insert` endpoint using the same `where`, `measure`, and `from` values.

```json
{
  "where": "LINE001",
  "measure": "nominal-rate",
  "target": 440,
  "unit": "pcs/min",
  "from": "2026-01-01T00:00:00+01:00",
  "setBy": "Line capability study"
}
```

The submitted values replace the values previously recorded for that Target.

### List targets

`GET /services/pulse/food-beverage/targets/query`

Returns Targets matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/targets/query?wheres=LINE001&measures=nominal-rate
```

Targets can also be filtered by their `from` timestamp:

```http
GET /services/pulse/food-beverage/targets/query?wheres=LINE001&start=2026-01-01T00:00:00%2B01:00&end=2026-12-31T23:59:59%2B01:00
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `wheres` | string or array of strings | no | Limits results to the specified production-resource business codes. |
| `measures` | string or array of strings | no | Limits results to the specified measurement business codes. |
| `start` | string | no | Earliest `from` timestamp to include. |
| `end` | string | no | Latest `from` timestamp to include. |

#### Example response

```json
[
  {
    "where": "LINE001",
    "measure": "nominal-rate",
    "min": null,
    "target": 420,
    "max": null,
    "unit": "pcs/min",
    "from": "2026-01-01T00:00:00+01:00",
    "setBy": "Line capability study"
  }
]
```