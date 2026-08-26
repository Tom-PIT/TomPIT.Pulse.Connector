# Site

Represents a physical operating location in Pulse.

A site is the root of the manufacturing hierarchy. Production lines belong to a site.

## The Site object

```json
{
  "code": "PLT001",
  "name": "Munich"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Unique business code used to identify the site in external systems and integrations. | `"PLT001"` |
| `name` | string | Human-readable name of the site. | `"Munich"` |

</div>

> [!IMPORTANT]
> `code` must be unique. Two sites cannot use the same code.

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## API resource

| Resource | Base path |
| --- | --- |
| `Site` | `/services/pulse/food-beverage/plants` |

## API methods

### Create a site

`POST /services/pulse/food-beverage/plants/insert`

Creates a new site.

#### Request

```http
POST /services/pulse/food-beverage/plants/insert
Content-Type: application/json
```

```json
{
  "code": "PLT001",
  "name": "Munich"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the site. |
| `name` | string | yes | Human-readable name of the site. |


### Update a site

`PUT /services/pulse/food-beverage/plants/update`

Updates an existing site.

#### Request

```http
PUT /services/pulse/food-beverage/plants/update
Content-Type: application/json
```

```json
{
  "code": "PLT001",
  "name": "Munich production site"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the site to update. |
| `name` | string | yes | Human-readable name of the site. |


### Patch a site

`PATCH /services/pulse/food-beverage/plants/patch`

Partially updates an existing site.

The fields to update are supplied in the `properties` object. The site is identified by its business `code`.

#### Request

```http
PATCH /services/pulse/food-beverage/plants/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "PLT001",
    "name": "Munich production site"
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Unique business code of the site to update. |
| `properties.name` | string | yes | Human-readable name of the site. |


### Retrieve a site

`GET /services/pulse/food-beverage/plants/select`

Returns the site identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/plants/select?id=PLT001
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the site to retrieve. |

#### Example response

```json
{
  "code": "PLT001",
  "name": "Munich"
}
```


### List sites

`GET /services/pulse/food-beverage/plants/query`

Returns sites matching the supplied filters.

Sites can be filtered by code and name.

#### Request

```http
GET /services/pulse/food-beverage/plants/query?codes=PLT001&names=Munich
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `codes` | string or array of strings | no | Limits results to sites with the specified business codes. |
| `names` | string or array of strings | no | Limits results to sites with the specified names. |

#### Example response

```json
[
  {
    "code": "PLT001",
    "name": "Munich"
  }
]
```


### Delete a site

`DELETE /services/pulse/food-beverage/plants/delete`

Deletes the site identified by its business code.

#### Request

```http
DELETE /services/pulse/food-beverage/plants/delete?id=PLT001
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the site to delete. |