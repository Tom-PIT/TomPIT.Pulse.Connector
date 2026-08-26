# Clean regime

Represents a defined cleaning regime used to classify comparable cleaning work in Food & Beverage operations.

Examples include allergen cleaning, full CIP, dry cleaning, or other repeatable cleaning procedures.

## The Clean regime object

```json
{
  "code": "allergen-cip",
  "name": "Allergen CIP"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Unique business code used to identify the cleaning regime. | `"allergen-cip"` |
| `name` | string | Human-readable name of the cleaning regime. | `"Allergen CIP"` |

</div>

> [!IMPORTANT]
> `code` must be unique. Two cleaning regimes cannot use the same code.

## API resource

| Resource | Base path |
| --- | --- |
| `Clean regime` | `/services/pulse/food-beverage/clean-regimes` |

## API methods

### Create a clean regime

`POST /services/pulse/food-beverage/clean-regimes/insert`

Creates a new cleaning regime.

#### Request

```http
POST /services/pulse/food-beverage/clean-regimes/insert
Content-Type: application/json
```

```json
{
  "code": "allergen-cip",
  "name": "Allergen CIP"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the cleaning regime. |
| `name` | string | yes | Human-readable name of the cleaning regime. |

### Update a clean regime

`PUT /services/pulse/food-beverage/clean-regimes/update`

Updates an existing cleaning regime.

#### Request

```http
PUT /services/pulse/food-beverage/clean-regimes/update
Content-Type: application/json
```

```json
{
  "code": "allergen-cip",
  "name": "Validated allergen CIP"
}
```

### Patch a clean regime

`PATCH /services/pulse/food-beverage/clean-regimes/patch`

Partially updates an existing cleaning regime.

The fields to update are supplied in the `properties` object. The cleaning regime is identified by its business `code`.

#### Request

```http
PATCH /services/pulse/food-beverage/clean-regimes/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "allergen-cip",
    "name": "Validated allergen CIP"
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Unique business code of the cleaning regime to update. |
| `properties.name` | string | no | New human-readable name of the cleaning regime. |

### Retrieve a clean regime

`GET /services/pulse/food-beverage/clean-regimes/select`

Returns the cleaning regime identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/clean-regimes/select?id=allergen-cip
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the cleaning regime to retrieve. |

#### Example response

```json
{
  "code": "allergen-cip",
  "name": "Allergen CIP"
}
```

### List clean regimes

`GET /services/pulse/food-beverage/clean-regimes/query`

Returns cleaning regimes matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/clean-regimes/query?names=Allergen%20CIP
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `codes` | string or array of strings | no | Limits results to cleaning regimes with the specified business codes. |
| `names` | string or array of strings | no | Limits results to cleaning regimes with the specified names. |

#### Example response

```json
[
  {
    "code": "allergen-cip",
    "name": "Allergen CIP"
  }
]
```

### Delete a clean regime

`DELETE /services/pulse/food-beverage/clean-regimes/delete`

Deletes the cleaning regime identified by its business code.

#### Request

```http
DELETE /services/pulse/food-beverage/clean-regimes/delete?id=allergen-cip
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the cleaning regime to delete. |