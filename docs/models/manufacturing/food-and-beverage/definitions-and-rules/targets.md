<!-- TODO: This page is currently based on ApiSurfaceRevised. Revisit it once the implementation is available and verify fields, routes, query parameters, PATCH behavior, and examples against the current code. -->

# Targets

Defines expected operating values for production lines, machines, vessels, rooms, and similar production subjects.

Targets apply from a specific point in time and can define a minimum, target, maximum, or any combination of those values.

## The Target object

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

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `where` | string | Business code of the line, machine, vessel, or room to which the target applies. | `"LINE001"` |
| [`measure`](measurements.md) | string | Measurement code that the target applies to. | `"nominal-rate"` |
| `min` | number or null | Optional minimum expected value. | `2` |
| `target` | number or null | Optional target value. | `4` |
| `max` | number or null | Optional maximum expected value. | `6` |
| `unit` | string | Unit of the submitted values. Used to validate the values against the measurement definition. | `"C"` |
| `from` | string | ISO 8601 timestamp from which the target is in force. | `"2026-01-01T00:00:00+01:00"` |
| `setBy` | string or null | Optional source or authority that established the target. | `"Chill chain policy"` |

</div>

> [!IMPORTANT]
> At least one of `min`, `target`, or `max` must be provided.
>
> `where` must reference an existing supported production subject and `measure` must reference an existing measurement.
>
> `unit` must be compatible with the unit declared for the measurement.

See [Types and attributes](../master-data/types-and-attributes.md) for guidance on extensible master-data properties.

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

An expected operating range for a cold room:

```json
{
  "where": "ROOM-01",
  "measure": "cold-room-temp",
  "min": 2,
  "target": 4,
  "max": 6,
  "unit": "C",
  "from": "2026-01-01T00:00:00+01:00",
  "setBy": "Chill chain policy"
}
```

## Effective dates

A target is identified by the combination of:

- `where`
- `measure`
- `from`

Use a new `from` value when an operating target changes.

For example, if a line's nominal rate changes on 1 July, register a new target starting on that date rather than changing the target that applied before July.

This preserves the operating expectation that was in force for earlier production.

## Targets are not alarms

Targets provide operating context for analysis.

A reading outside a target range does not by itself create an event or alarm. Targets allow Pulse to compare actual operation with the value or range that was expected at that time.

## API resource

| Resource | Base path |
| --- | --- |
| `Target` | `/services/pulse/food-beverage/targets` |

## API methods

> [!NOTE]
> The API methods below follow the current Food & Beverage service pattern and are provisional until the Targets implementation is available for verification.

### Create a target

`POST /services/pulse/food-beverage/targets/insert`

Creates a new target.

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
| `where` | string | yes | Business code of the subject to which the target applies. |
| `measure` | string | yes | Measurement code that the target applies to. |
| `min` | number or null | no | Minimum expected value. |
| `target` | number or null | no | Target value. |
| `max` | number or null | no | Maximum expected value. |
| `unit` | string | yes | Unit used to validate the submitted values. |
| `from` | string | yes | ISO 8601 timestamp from which the target is in force. |
| `setBy` | string or null | no | Source or authority that established the target. |


### Update a target

`PUT /services/pulse/food-beverage/targets/update`

Updates an existing target revision.

The target is identified by its subject, measurement, and effective-from timestamp.

#### Request

```http
PUT /services/pulse/food-beverage/targets/update
Content-Type: application/json
```

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


### Patch a target

`PATCH /services/pulse/food-beverage/targets/patch`

Partially updates an existing target revision.

The exact PATCH identification shape still needs to be verified against the implementation.

#### Request

```http
PATCH /services/pulse/food-beverage/targets/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "where": "LINE001",
    "measure": "nominal-rate",
    "from": "2026-01-01T00:00:00+01:00",
    "target": 440
  }
}
```


### Retrieve a target

`GET /services/pulse/food-beverage/targets/select`

Returns a target revision.

The facade specification defines a stable key derived from `(where, measure, from)`.

#### Request

```http
GET /services/pulse/food-beverage/targets/select?id=LINE001/nominal-rate/2026-01-01
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Stable target code derived from subject, measurement, and effective date. |


### List targets

`GET /services/pulse/food-beverage/targets/query`

Returns targets matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/targets/query?where=LINE001&measure=nominal-rate
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `where` | string | no | Limits results to a specific subject. |
| `measure` | string | no | Limits results to a specific measurement. |
| `inForceAt` | string | no | Limits results to the target in force at the specified time. |


### Delete a target

`DELETE /services/pulse/food-beverage/targets/delete`

Deletes the specified target revision.

#### Request

```http
DELETE /services/pulse/food-beverage/targets/delete?id=LINE001/nominal-rate/2026-01-01
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Stable target code derived from subject, measurement, and effective date. |