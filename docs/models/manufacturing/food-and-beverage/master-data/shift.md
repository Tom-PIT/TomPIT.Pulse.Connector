# Shift

Represents a production shift used as calendar context for Food & Beverage operations.

Shifts allow operational results to be compared across recurring working periods, such as day, evening, or night shifts.

## The Shift object

```json
{
  "code": "SHIFT_C",
  "name": "Night shift"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Unique business code used to identify the shift in external systems and integrations. | `"SHIFT_C"` |
| `name` | string | Human-readable name of the shift. | `"Night shift"` |

</div>

> [!IMPORTANT]
> `code` must be unique. Two shifts cannot use the same code.

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## API resource

| Resource | Base path |
| --- | --- |
| `Shift` | `/services/pulse/food-beverage/shifts` |

## API methods

### Create a shift

`POST /services/pulse/food-beverage/shifts/insert`

Creates a new shift.

#### Request

```http
POST /services/pulse/food-beverage/shifts/insert
Content-Type: application/json
```

```json
{
  "code": "SHIFT_C",
  "name": "Night shift"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the shift. |
| `name` | string | yes | Human-readable name of the shift. |


### Update a shift

`PUT /services/pulse/food-beverage/shifts/update`

Updates an existing shift.

#### Request

```http
PUT /services/pulse/food-beverage/shifts/update
Content-Type: application/json
```

```json
{
  "code": "SHIFT_C",
  "name": "Night shift"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the shift to update. |
| `name` | string | yes | Human-readable name of the shift. |


### Patch a shift

`PATCH /services/pulse/food-beverage/shifts/patch`

Partially updates an existing shift.

The fields to update are supplied in the `properties` object. The shift is identified by its business `code`.

#### Request

```http
PATCH /services/pulse/food-beverage/shifts/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "SHIFT_C",
    "name": "Night shift 3"
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Unique business code of the shift to update. |
| `properties.name` | string | no | New human-readable name of the shift. |


### Retrieve a shift

`GET /services/pulse/food-beverage/shifts/select`

Returns the shift identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/shifts/select?id=SHIFT_C
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the shift to retrieve. |

#### Example response

```json
{
  "code": "SHIFT_C",
  "name": "Night shift"
}
```


### List shifts

`GET /services/pulse/food-beverage/shifts/query`

Returns shifts matching the supplied filters.

Shifts can be filtered by code and name.

#### Request

```http
GET /services/pulse/food-beverage/shifts/query?names=Night%20shift
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `codes` | string or array of strings | no | Limits results to shifts with the specified business codes. |
| `names` | string or array of strings | no | Limits results to shifts with the specified names. |

#### Example response

```json
[
  {
    "code": "SHIFT_C",
    "name": "Night shift"
  }
]
```


### Delete a shift

`DELETE /services/pulse/food-beverage/shifts/delete`

Deletes the shift identified by its business code.

#### Request

```http
DELETE /services/pulse/food-beverage/shifts/delete?id=SHIFT_C
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the shift to delete. |