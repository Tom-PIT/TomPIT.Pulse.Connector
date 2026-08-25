# Crew

Represents a production crew responsible for Food & Beverage operations.

Crews provide operational context for labour and production activity without identifying individual people.

## The Crew object

```json
{
  "code": "CREW-C",
  "name": "Night crew C"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Unique business code used to identify the crew in external systems and integrations. | `"CREW-C"` |
| `name` | string | Human-readable name of the crew. | `"Night crew C"` |

</div>

> [!IMPORTANT]
> `code` must be unique. Two crews cannot use the same code.

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## API resource

| Resource | Base path |
| --- | --- |
| `Crew` | `/services/pulse/food-beverage/crews` |

## API methods

### Create a crew

`POST /services/pulse/food-beverage/crews/insert`

Creates a new crew.

#### Request

```http
POST /services/pulse/food-beverage/crews/insert
Content-Type: application/json
```

```json
{
  "code": "CREW-C",
  "name": "Night crew C"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the crew. |
| `name` | string | yes | Human-readable name of the crew. |


### Update a crew

`PUT /services/pulse/food-beverage/crews/update`

Updates an existing crew.

#### Request

```http
PUT /services/pulse/food-beverage/crews/update
Content-Type: application/json
```

```json
{
  "code": "CREW-C",
  "name": "Night crew C"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the crew to update. |
| `name` | string | yes | Human-readable name of the crew. |


### Patch a crew

`PATCH /services/pulse/food-beverage/crews/patch`

Partially updates an existing crew.

The fields to update are supplied in the `properties` object. The crew is identified by its business `code`.

#### Request

```http
PATCH /services/pulse/food-beverage/crews/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "CREW-C",
    "name": "Night crew C2"
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Unique business code of the crew to update. |
| `properties.name` | string | no | New human-readable name of the crew. |


### Retrieve a crew

`GET /services/pulse/food-beverage/crews/select`

Returns the crew identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/crews/select?id=CREW-C
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the crew to retrieve. |

#### Example response

```json
{
  "code": "CREW-C",
  "name": "Night crew C"
}
```


### List crews

`GET /services/pulse/food-beverage/crews/query`

Returns crews matching the supplied filters.

Crews can be filtered by code and name.

#### Request

```http
GET /services/pulse/food-beverage/crews/query?names=Night%20crew%20C
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `codes` | string or array of strings | no | Limits results to crews with the specified business codes. |
| `names` | string or array of strings | no | Limits results to crews with the specified names. |

#### Example response

```json
[
  {
    "code": "CREW-C",
    "name": "Night crew C"
  }
]
```


### Delete a crew

`DELETE /services/pulse/food-beverage/crews/delete`

Deletes the crew identified by its business code.

#### Request

```http
DELETE /services/pulse/food-beverage/crews/delete?id=CREW-C
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the crew to delete. |