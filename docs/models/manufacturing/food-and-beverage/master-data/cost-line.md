# Cost line

Represents a non-material cost that can be assigned to production or maintenance activity.

Examples include overtime, subcontracting, disposal, or other operational charges.

## The Cost line object

```json
{
  "code": "EXP-OVERTIME",
  "name": "Overtime premium",
  "unit": "h"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Unique business code used to identify the cost line in external systems and integrations. | `"EXP-OVERTIME"` |
| `name` | string | Human-readable name of the cost line. | `"Overtime premium"` |
| `unit` | string or null | Optional unit used to quantify the cost line. | `"h"` |

</div>

> [!IMPORTANT]
> `code` must be unique. Two cost lines cannot use the same code.

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## API resource

| Resource | Base path |
| --- | --- |
| `Cost line` | `/services/pulse/food-beverage/cost-lines` |

## API methods

### Create a cost line

`POST /services/pulse/food-beverage/cost-lines/insert`

Creates a new cost line.

#### Request

```http
POST /services/pulse/food-beverage/cost-lines/insert
Content-Type: application/json
```

```json
{
  "code": "EXP-OVERTIME",
  "name": "Overtime premium",
  "unit": "h"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the cost line. |
| `name` | string | yes | Human-readable name of the cost line. |
| `unit` | string or null | no | Optional unit used to quantify the cost line. |


### Update a cost line

`PUT /services/pulse/food-beverage/cost-lines/update`

Updates an existing cost line.

#### Request

```http
PUT /services/pulse/food-beverage/cost-lines/update
Content-Type: application/json
```

```json
{
  "code": "EXP-OVERTIME",
  "name": "Overtime premium",
  "unit": "h"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the cost line to update. |
| `name` | string | yes | Human-readable name of the cost line. |
| `unit` | string or null | no | Optional unit used to quantify the cost line. |


### Patch a cost line

`PATCH /services/pulse/food-beverage/cost-lines/patch`

Partially updates an existing cost line.

The fields to update are supplied in the `properties` object. The cost line is identified by its business `code`.

#### Request

```http
PATCH /services/pulse/food-beverage/cost-lines/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "EXP-OVERTIME",
    "name": "Overtime premium – weekend",
    "unit": "h"
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Unique business code of the cost line to update. |
| `properties.name` | string | no | New human-readable name of the cost line. |
| `properties.unit` | string or null | no | New unit used to quantify the cost line. |


### Retrieve a cost line

`GET /services/pulse/food-beverage/cost-lines/select`

Returns the cost line identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/cost-lines/select?id=EXP-OVERTIME
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the cost line to retrieve. |

#### Example response

```json
{
  "code": "EXP-OVERTIME",
  "name": "Overtime premium",
  "unit": "h"
}
```


### List cost lines

`GET /services/pulse/food-beverage/cost-lines/query`

Returns cost lines matching the supplied filters.

Cost lines can be filtered by code, name, and unit.

#### Request

```http
GET /services/pulse/food-beverage/cost-lines/query?units=h
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `codes` | string or array of strings | no | Limits results to cost lines with the specified business codes. |
| `names` | string or array of strings | no | Limits results to cost lines with the specified names. |
| `units` | string or array of strings | no | Limits results to cost lines with the specified units. |

#### Example response

```json
[
  {
    "code": "EXP-OVERTIME",
    "name": "Overtime premium",
    "unit": "h"
  },
  {
    "code": "EXP-SUBCONTRACT",
    "name": "Subcontracting",
    "unit": "job"
  }
]
```


### Delete a cost line

`DELETE /services/pulse/food-beverage/cost-lines/delete`

Deletes the cost line identified by its business code.

#### Request

```http
DELETE /services/pulse/food-beverage/cost-lines/delete?id=EXP-OVERTIME
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the cost line to delete. |