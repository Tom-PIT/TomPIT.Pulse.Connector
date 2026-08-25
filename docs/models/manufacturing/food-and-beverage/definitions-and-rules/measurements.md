<!-- TODO: This page is currently based on ApiSurfaceRevised. Revisit it once the implementation is available and verify fields, routes, query parameters, PATCH behavior, and examples against the current code. -->

# Measurements

Defines the measurable or commanded values that can be recorded in Food & Beverage operations.

A measurement definition describes what a value means, how Pulse should interpret it, and whether the value represents an observed measurement or a commanded setpoint.

Measurement definitions must exist before values are submitted through [Readings](../operational-data/readings.md) or [Settings](../operational-data/settings.md).

## The Measurement object

```json
{
  "code": "freezer-door",
  "name": "Freezer door open",
  "unit": "",
  "valueType": "Boolean",
  "semanticType": "Ambient",
  "parameterClass": "Measurement",
  "aggregation": "Mode",
  "capture": "EventOnChange",
  "cadenceMinutes": null,
  "plausibleMin": null,
  "plausibleMax": null
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Unique business code used when recording values for the measurement. | `"freezer-door"` |
| `name` | string | Human-readable name of the measurement. | `"Freezer door open"` |
| `unit` | string | Unit in which values are expressed. Can be empty for Boolean or categorical values. | `""` |
| `valueType` | string | Defines the kind of value carried by the measurement. Supported values are `Continuous`, `Ordinal`, `Categorical`, and `Boolean`. | `"Boolean"` |
| `aggregation` | string | Defines how multiple values are aggregated. | `"Mode"` |
| `semanticType` | string | Describes the business meaning of the measurement. | `"Ambient"` |
| `parameterClass` | string | Indicates whether values represent measurements or commanded setpoints. Supported values are `Measurement` and `Setpoint`. | `"Measurement"` |
| `capture` | string | Defines how values are captured. Supported values are `Sampled` and `EventOnChange`. | `"EventOnChange"` |
| `cadenceMinutes` | number or null | Expected interval, in minutes, between sampled values. Omit when values are recorded only when they change. | `5` |
| `plausibleMin` | number or null | Optional lower bound below which a captured value is considered implausible. | `-40` |
| `plausibleMax` | number or null | Optional upper bound above which a captured value is considered implausible. | `60` |

</div>

> [!IMPORTANT]
> `code` must be unique.
>
> Enumerated values must use the supported value exactly. For example, use `Measurement`, not `Measured`.

See [Types and attributes](../master-data/types-and-attributes.md) for guidance on extensible master-data properties.

## Value types

Use `valueType` to describe the shape of values submitted for the measurement.

| Value | Use for |
| --- | --- |
| `Continuous` | Numeric values such as temperature, pressure, weight, speed, or cost |
| `Ordinal` | Ordered categories such as low, normal, and high |
| `Categorical` | Unordered categories such as operating mode or selector position |
| `Boolean` | Two-state values such as open/closed or on/off |

## Measurement and setpoint values

The `parameterClass` field determines what kind of value the definition represents.

`Measurement` represents something observed from the process or equipment.

For example:

```json
{
  "code": "product-temp",
  "name": "Product temperature",
  "unit": "C",
  "valueType": "Continuous",
  "semanticType": "Quality",
  "parameterClass": "Measurement",
  "aggregation": "Avg",
  "capture": "Sampled"
}
```

Values for these definitions are submitted through [Readings](../operational-data/readings.md).

`Setpoint` represents a value commanded by a person or control system.

For example:

```json
{
  "code": "fill-target",
  "name": "Fill target",
  "unit": "g",
  "valueType": "Continuous",
  "semanticType": "Quality",
  "parameterClass": "Setpoint",
  "aggregation": "Last",
  "capture": "EventOnChange"
}
```

Values for these definitions are submitted through [Settings](../operational-data/settings.md).

Measured values and setpoints are kept separate because what a machine was commanded to do and what it actually did are different facts.

## Capture modes

`Sampled` measurements are recorded repeatedly on a cadence, such as a temperature reading every five minutes.

`EventOnChange` measurements are recorded only when their value changes, such as a door opening or a setpoint being changed.

There is no `Derived` capture mode. Values calculated from other measurements are produced by Pulse rather than submitted as captured values.

## Plausible bounds

`plausibleMin` and `plausibleMax` identify values that are physically or technically implausible.

For example, a temperature sensor reporting `900 C` may indicate a broken instrument rather than a valid process measurement.

Plausible bounds are not process targets or product specification limits.

Use [Product limits](product-limits.md) or [Targets](targets.md) for expected or permitted operating ranges.

## API resource

| Resource | Base path |
| --- | --- |
| `Measurement` | `/services/pulse/food-beverage/measurements` |

## API methods

> [!NOTE]
> The API methods below follow the current Food & Beverage service pattern and are provisional until the Measurements implementation is available for verification.

### Create a measurement

`POST /services/pulse/food-beverage/measurements/insert`

Creates a new measurement definition.

#### Request

```http
POST /services/pulse/food-beverage/measurements/insert
Content-Type: application/json
```

```json
{
  "code": "freezer-door",
  "name": "Freezer door open",
  "unit": "",
  "valueType": "Boolean",
  "semanticType": "Ambient",
  "parameterClass": "Measurement",
  "aggregation": "Mode",
  "capture": "EventOnChange",
  "cadenceMinutes": null,
  "plausibleMin": null,
  "plausibleMax": null
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the measurement. |
| `name` | string | yes | Human-readable name of the measurement. |
| `unit` | string | no | Unit in which values are expressed. |
| `valueType` | string | yes | Kind of value carried by the measurement. |
| `semanticType` | string | yes | Business meaning of the measurement. |
| `parameterClass` | string | yes | `Measurement` or `Setpoint`. |
| `aggregation` | string | yes | Aggregation method used for multiple values. |
| `capture` | string | yes | `Sampled` or `EventOnChange`. |
| `cadenceMinutes` | number or null | no | Expected interval between sampled values. |
| `plausibleMin` | number or null | no | Optional lower plausible bound. |
| `plausibleMax` | number or null | no | Optional upper plausible bound. |


### Update a measurement

`PUT /services/pulse/food-beverage/measurements/update`

Updates an existing measurement definition.

#### Request

```http
PUT /services/pulse/food-beverage/measurements/update
Content-Type: application/json
```

```json
{
  "code": "freezer-door",
  "name": "Freezer door status",
  "unit": "",
  "valueType": "Boolean",
  "semanticType": "Ambient",
  "parameterClass": "Measurement",
  "aggregation": "Mode",
  "capture": "EventOnChange",
  "cadenceMinutes": null,
  "plausibleMin": null,
  "plausibleMax": null
}
```


### Patch a measurement

`PATCH /services/pulse/food-beverage/measurements/patch`

Partially updates an existing measurement definition.

The fields to update are supplied in the `properties` object. The measurement is identified by its business `code`.

#### Request

```http
PATCH /services/pulse/food-beverage/measurements/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "freezer-door",
    "name": "Freezer door status",
    "capture": "EventOnChange"
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Unique business code of the measurement to update. |
| `properties.name` | string | no | New human-readable name. |
| `properties.unit` | string or null | no | New unit. |
| `properties.valueType` | string | no | New value type. |
| `properties.semanticType` | string | no | New semantic type. |
| `properties.parameterClass` | string | no | New parameter class. |
| `properties.aggregation` | string | no | New aggregation method. |
| `properties.capture` | string | no | New capture mode. |
| `properties.cadenceMinutes` | number or null | no | New expected cadence. |
| `properties.plausibleMin` | number or null | no | New lower plausible bound. |
| `properties.plausibleMax` | number or null | no | New upper plausible bound. |


### Retrieve a measurement

`GET /services/pulse/food-beverage/measurements/select`

Returns the measurement identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/measurements/select?id=freezer-door
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the measurement to retrieve. |

#### Example response

```json
{
  "code": "freezer-door",
  "name": "Freezer door open",
  "unit": "",
  "valueType": "Boolean",
  "semanticType": "Ambient",
  "parameterClass": "Measurement",
  "aggregation": "Mode",
  "capture": "EventOnChange",
  "cadenceMinutes": null,
  "plausibleMin": null,
  "plausibleMax": null
}
```


### List measurements

`GET /services/pulse/food-beverage/measurements/query`

Returns measurement definitions matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/measurements/query?parameterClasses=Measurement&semanticTypes=Ambient
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `codes` | string or array of strings | no | Limits results to measurements with the specified business codes. |
| `names` | string or array of strings | no | Limits results to measurements with the specified names. |
| `parameterClasses` | string or array of strings | no | Limits results to measurements with the specified parameter classes. |
| `semanticTypes` | string or array of strings | no | Limits results to measurements with the specified semantic types. |

#### Example response

```json
[
  {
    "code": "freezer-door",
    "name": "Freezer door open",
    "unit": "",
    "valueType": "Boolean",
    "semanticType": "Ambient",
    "parameterClass": "Measurement",
    "aggregation": "Mode",
    "capture": "EventOnChange",
    "cadenceMinutes": null,
    "plausibleMin": null,
    "plausibleMax": null
  }
]
```


### Delete a measurement

`DELETE /services/pulse/food-beverage/measurements/delete`

Deletes the measurement identified by its business code.

#### Request

```http
DELETE /services/pulse/food-beverage/measurements/delete?id=freezer-door
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the measurement to delete. |