# Production line

Represents a production line within a site.

A production line belongs to exactly one site.

## The Production line object

```json
{
  "code": "LINE001",
  "name": "Yogurt filling line 1",
  "parent": "PLT001"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Unique business code used to identify the production line in external systems and integrations. | `"LINE001"` |
| `name` | string | Human-readable name of the production line. | `"Yogurt filling line 1"` |
| [`parent`](site.md) | string | Business code of the site the production line belongs to. | `"PLT001"` |

</div>

> [!IMPORTANT]
> `code` must be unique. Two production lines cannot use the same code.
>
> The site referenced by `parent` must already exist before the production line is submitted.

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## API resource

| Resource | Base path |
| --- | --- |
| `Production line` | `/services/pulse/food-beverage/lines` |

## API methods

### Create a production line

`POST /services/pulse/food-beverage/lines/insert`

Creates a new production line.

#### Request

```http
POST /services/pulse/food-beverage/lines/insert
Content-Type: application/json
```

```json
{
  "code": "LINE001",
  "name": "Yogurt filling line 1",
  "parent": "PLT001"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the production line. |
| `name` | string | yes | Human-readable name of the production line. |
| `parent` | string | yes | Business code of the site the production line belongs to. |


### Update a production line

`PUT /services/pulse/food-beverage/lines/update`

Updates an existing production line.

#### Request

```http
PUT /services/pulse/food-beverage/lines/update
Content-Type: application/json
```

```json
{
  "code": "LINE001",
  "name": "Yogurt filling line 1",
  "parent": "PLT002"
}
```

Changing `parent` moves the production line to another site.

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the production line to update. |
| `name` | string | yes | Human-readable name of the production line. |
| `parent` | string | yes | Business code of the site the production line belongs to. |


### Patch a production line

`PATCH /services/pulse/food-beverage/lines/patch`

Partially updates an existing production line.

The fields to update are supplied in the `properties` object. The production line is identified by its business `code`.

#### Request

```http
PATCH /services/pulse/food-beverage/lines/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "LINE001",
    "name": "Yogurt filling line 1A",
    "parent": "PLT002"
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Unique business code of the production line to update. |
| `properties.name` | string | no | New human-readable name of the production line. |
| `properties.parent` | string | no | Business code of the site the production line should belong to. |


### Retrieve a production line

`GET /services/pulse/food-beverage/lines/select`

Returns the production line identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/lines/select?id=LINE001
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the production line to retrieve. |

#### Example response

```json
{
  "code": "LINE001",
  "name": "Yogurt filling line 1",
  "parent": "PLT001"
}
```


### List production lines

`GET /services/pulse/food-beverage/lines/query`

Returns production lines matching the supplied filters.

Production lines can be filtered by code, name, and parent site.

#### Request

```http
GET /services/pulse/food-beverage/lines/query?parents=PLT001
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `codes` | string or array of strings | no | Limits results to production lines with the specified business codes. |
| `names` | string or array of strings | no | Limits results to production lines with the specified names. |
| `parents` | string or array of strings | no | Limits results to production lines belonging to the specified sites. |

#### Example response

```json
[
  {
    "code": "LINE001",
    "name": "Yogurt filling line 1",
    "parent": "PLT001"
  },
  {
    "code": "LINE002",
    "name": "Yogurt filling line 2",
    "parent": "PLT001"
  }
]
```


### Delete a production line

`DELETE /services/pulse/food-beverage/lines/delete`

Deletes the production line identified by its business code.

#### Request

```http
DELETE /services/pulse/food-beverage/lines/delete?id=LINE001
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the production line to delete. |