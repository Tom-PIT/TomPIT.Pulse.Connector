# Recipe

Represents a versioned recipe used to produce a product.

A recipe can optionally reference the product it produces. Use a new unique recipe code for each recipe version.

## The Recipe object

```json
{
  "code": "REC-PRD001-v3",
  "name": "Yogurt Strawberry 125g v3",
  "parent": "PRD001"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Unique business code used to identify the recipe version in external systems and integrations. | `"REC-PRD001-v3"` |
| `name` | string | Human-readable name of the recipe. | `"Yogurt Strawberry 125g v3"` |
| [`parent`](product.md) | string or null | Business code of the product produced by the recipe. Can be omitted for a base or intermediate recipe. | `"PRD001"` |

</div>

> [!IMPORTANT]
> `code` must be unique. Use a new code for each recipe version rather than updating the code of an existing version.
>
> When `parent` is provided, the referenced product must already exist.

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## API resource

| Resource | Base path |
| --- | --- |
| `Recipe` | `/services/pulse/food-beverage/recipes` |

## API methods

### Create a recipe

`POST /services/pulse/food-beverage/recipes/insert`

Creates a new recipe.

#### Request

```http
POST /services/pulse/food-beverage/recipes/insert
Content-Type: application/json
```

```json
{
  "code": "REC-PRD001-v3",
  "name": "Yogurt Strawberry 125g v3",
  "parent": "PRD001"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the recipe version. |
| `name` | string | yes | Human-readable name of the recipe. |
| `parent` | string or null | no | Business code of the product produced by the recipe. |


### Update a recipe

`PUT /services/pulse/food-beverage/recipes/update`

Updates an existing recipe.

#### Request

```http
PUT /services/pulse/food-beverage/recipes/update
Content-Type: application/json
```

```json
{
  "code": "REC-PRD001-v3",
  "name": "Yogurt Strawberry 125g v3",
  "parent": "PRD001"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the recipe to update. |
| `name` | string | yes | Human-readable name of the recipe. |
| `parent` | string or null | no | Business code of the product produced by the recipe. |


### Patch a recipe

`PATCH /services/pulse/food-beverage/recipes/patch`

Partially updates an existing recipe.

The fields to update are supplied in the `properties` object. The recipe is identified by its business `code`.

#### Request

```http
PATCH /services/pulse/food-beverage/recipes/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "REC-PRD001-v3",
    "name": "Yogurt Strawberry 125g version 3",
    "parent": "PRD001"
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Unique business code of the recipe to update. |
| `properties.name` | string | no | New human-readable name of the recipe. |
| `properties.parent` | string or null | no | Business code of the product produced by the recipe. |


### Retrieve a recipe

`GET /services/pulse/food-beverage/recipes/select`

Returns the recipe identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/recipes/select?id=REC-PRD001-v3
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the recipe to retrieve. |

#### Example response

```json
{
  "code": "REC-PRD001-v3",
  "name": "Yogurt Strawberry 125g v3",
  "parent": "PRD001"
}
```


### List recipes

`GET /services/pulse/food-beverage/recipes/query`

Returns recipes matching the supplied filters.

Recipes can be filtered by code, name, and parent product.

#### Request

```http
GET /services/pulse/food-beverage/recipes/query?parents=PRD001
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `codes` | string or array of strings | no | Limits results to recipes with the specified business codes. |
| `names` | string or array of strings | no | Limits results to recipes with the specified names. |
| `parents` | string or array of strings | no | Limits results to recipes associated with the specified products. |

#### Example response

```json
[
  {
    "code": "REC-PRD001-v2",
    "name": "Yogurt Strawberry 125g v2",
    "parent": "PRD001"
  },
  {
    "code": "REC-PRD001-v3",
    "name": "Yogurt Strawberry 125g v3",
    "parent": "PRD001"
  }
]
```


### Delete a recipe

`DELETE /services/pulse/food-beverage/recipes/delete`

Deletes the recipe identified by its business code.

#### Request

```http
DELETE /services/pulse/food-beverage/recipes/delete?id=REC-PRD001-v3
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the recipe to delete. |