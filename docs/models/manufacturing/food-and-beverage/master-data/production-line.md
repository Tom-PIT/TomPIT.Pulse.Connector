# Production line

Represents a production line within a site.

A production line belongs to exactly one site.

## The Production line object

```json
{
  "code": "LINE001",
  "name": "Yogurt filling line 1",
  "site": "PLT001"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the production line in external systems and integrations. | `"LINE001"` |
| `name` | string | Human-readable name of the production line. | `"Yogurt filling line 1"` |
| [`site`](site.md) | string | Business code of the site the production line belongs to. | `"PLT001"` |

</div>

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## API resource

| Resource | Base path |
| --- | --- |
| `Production line` | `/services/pulse/food-beverage/lines` |

## API methods

### Create a production line

`POST /services/pulse/food-beverage/lines`

Creates a new production line.

#### Request

```http
POST /services/pulse/food-beverage/lines
Content-Type: application/json
```

```json
{
  "code": "LINE001",
  "name": "Yogurt filling line 1",
  "site": "PLT001"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Business code of the production line. |
| `name` | string | yes | Human-readable name of the production line. |
| `site` | string | yes | Business code of the site the production line belongs to. |


### Update a production line

`PUT /services/pulse/food-beverage/lines`

Updates an existing production line.

#### Request

```http
PUT /services/pulse/food-beverage/lines
Content-Type: application/json
```

```json
{
  "code": "LINE001",
  "name": "Yogurt filling line 1",
  "site": "PLT002"
}
```

Changing `site` moves the production line to another site.

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Business code of the production line to update. |
| `name` | string | yes | Human-readable name of the production line. |
| `site` | string | yes | Business code of the site the production line belongs to. |


### Patch a production line

`PATCH /services/pulse/food-beverage/lines`

Partially updates an existing production line.

The fields to update are supplied in the `properties` object.

#### Request

```http
PATCH /services/pulse/food-beverage/lines
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "LINE001",
    "name": "Yogurt filling line 1A",
    "site": "PLT002"
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Business code of the production line to update. |
| `properties.name` | string | no | New human-readable name of the production line. |
| `properties.site` | string | no | Business code of the site the production line should belong to. |


### Retrieve a production line

`GET /services/pulse/food-beverage/lines/{code}`

Returns the production line identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/lines/LINE001
```

#### Example response

```json
{
  "code": "LINE001",
  "name": "Yogurt filling line 1",
  "site": "PLT001"
}
```


### List production lines

`GET /services/pulse/food-beverage/lines`

Returns production lines matching the supplied filters.

Production lines can be filtered by site.

#### Request

```http
GET /services/pulse/food-beverage/lines?site=PLT001
```

#### Query parameters

| Parameter | Type | Description |
| --- | --- | --- |
| `site` | string | Limits results to production lines belonging to the specified site. |

#### Example response

```json
[
  {
    "code": "LINE001",
    "name": "Yogurt filling line 1",
    "site": "PLT001"
  },
  {
    "code": "LINE002",
    "name": "Yogurt filling line 2",
    "site": "PLT001"
  }
]
```


### Delete a production line

`DELETE /services/pulse/food-beverage/lines/{code}`

Deletes the production line identified by its business code.

#### Request

```http
DELETE /services/pulse/food-beverage/lines/LINE001
```