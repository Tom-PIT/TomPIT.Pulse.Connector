# Machine

Represents a machine, equipment asset, component, or sensor in Pulse.

Machines can be organised hierarchically. A machine can belong to another machine, a production line, or a site.

## The Machine object

```json
{
  "code": "EQ010-H06",
  "name": "Filler 1 head 6",
  "parent": "EQ010",
  "measures": "fill-weight"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the machine in external systems and integrations. | `"EQ010-H06"` |
| `name` | string | Human-readable name of the machine. | `"Filler 1 head 6"` |
| `parent` | string | Business code of the machine, production line, or site that contains this machine. | `"EQ010"` |
| [`measures`](measurement.md) | string or null | Metric code produced by the machine when it represents a sensor or probe. | `"fill-weight"` |

</div>

> [!IMPORTANT]
> The entity referenced by `parent` must already exist before the machine is submitted. `parent` can reference another machine, a production line, or a site.

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## Machine hierarchy

Machines can contain other machines or components with no fixed hierarchy depth.

For example:

```text
Production line
└── Filler
    └── Filler head 6
        └── Nozzle
```

Each machine references the entity directly above it through `parent`.

A machine can also belong directly to a site. For example, a cold room does not need to belong to a production line.

## Sensors

A machine can also represent a sensor or probe.

When the machine produces a measurement, `measures` identifies the metric it reads:

```json
{
  "code": "TEMP-PROBE-01",
  "name": "Cold room temperature probe",
  "parent": "COLD-ROOM-01",
  "measures": "temperature"
}
```

For machines that do not produce measurements, `measures` can be omitted or set to `null`.

## API resource

| Resource | Base path |
| --- | --- |
| `Machine` | `/services/pulse/food-beverage/machines` |

## API methods

### Create a machine

`POST /services/pulse/food-beverage/machines`

Creates a new machine, component, or sensor.

#### Request

```http
POST /services/pulse/food-beverage/machines
Content-Type: application/json
```

```json
{
  "code": "EQ010-H06",
  "name": "Filler 1 head 6",
  "parent": "EQ010",
  "measures": "fill-weight"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Business code of the machine. |
| `name` | string | yes | Human-readable name of the machine. |
| `parent` | string | yes | Business code of the machine, production line, or site that contains the machine. |
| `measures` | string or null | no | Metric code produced by the machine when it represents a sensor or probe. |


### Update a machine

`PUT /services/pulse/food-beverage/machines`

Updates an existing machine.

#### Request

```http
PUT /services/pulse/food-beverage/machines
Content-Type: application/json
```

```json
{
  "code": "EQ010-H06",
  "name": "Filler 1 head 6",
  "parent": "EQ010",
  "measures": "fill-weight"
}
```

Changing `parent` moves the machine within the equipment hierarchy.

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Business code of the machine to update. |
| `name` | string | yes | Human-readable name of the machine. |
| `parent` | string | yes | Business code of the machine, production line, or site that contains the machine. |
| `measures` | string or null | no | Metric code produced by the machine when it represents a sensor or probe. |


### Patch a machine

`PATCH /services/pulse/food-beverage/machines`

Partially updates an existing machine.

The fields to update are supplied in the `properties` object. The machine is identified by its business `code`.

#### Request

```http
PATCH /services/pulse/food-beverage/machines
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "EQ010-H06",
    "name": "Filler head 6",
    "parent": "EQ010",
    "measures": "fill-weight"
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Business code of the machine to update. |
| `properties.name` | string | no | New human-readable name of the machine. |
| `properties.parent` | string | no | New parent machine, production line, or site code. |
| `properties.measures` | string or null | no | New metric code produced by the machine. |


### Retrieve a machine

`GET /services/pulse/food-beverage/machines/{code}`

Returns the machine identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/machines/EQ010-H06
```

#### Example response

```json
{
  "code": "EQ010-H06",
  "name": "Filler 1 head 6",
  "parent": "EQ010",
  "measures": "fill-weight"
}
```


### List machines

`GET /services/pulse/food-beverage/machines`

Returns machines matching the supplied filters.

Machines can be filtered by code, name, and parent.

#### Request

```http
GET /services/pulse/food-beverage/machines?parent=EQ010
```

#### Query parameters

| Parameter | Type | Description |
| --- | --- | --- |
| `code` | string | Limits results to the specified machine code. |
| `name` | string | Limits results to the specified machine name. |
| `parent` | string | Limits results to machines with the specified parent. |

#### Example response

```json
[
  {
    "code": "EQ010-H05",
    "name": "Filler 1 head 5",
    "parent": "EQ010",
    "measures": null
  },
  {
    "code": "EQ010-H06",
    "name": "Filler 1 head 6",
    "parent": "EQ010",
    "measures": "fill-weight"
  }
]
```


### Delete a machine

`DELETE /services/pulse/food-beverage/machines/{code}`

Deletes the machine identified by its business code.

#### Request

```http
DELETE /services/pulse/food-beverage/machines/EQ010-H06
```