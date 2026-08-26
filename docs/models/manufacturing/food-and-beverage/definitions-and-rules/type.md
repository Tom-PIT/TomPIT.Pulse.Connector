# Type

Defines a controlled classification that can be applied to a Material or Product.

Types are used for classifications such as allergens, storage conditions, origin, or pack format. Each Type defines a set of allowed values that can be referenced from supported master-data fields.

## The Type object

```json
{
  "code": "allergen",
  "name": "Allergen",
  "appliesTo": "material",
  "values": [
    {
      "code": "ALG-MILK",
      "name": "Milk"
    },
    {
      "code": "ALG-NONE",
      "name": "None"
    }
  ]
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Unique business code used to identify the Type. | `"allergen"` |
| `name` | string | Human-readable name of the Type. | `"Allergen"` |
| `appliesTo` | string | Resource the classification applies to. Supported values are `material` and `product`. | `"material"` |
| `values` | array | Classification values registered for the Type. | |
| `values[].code` | string | Business code used to identify the value within the Type. | `"ALG-MILK"` |
| `values[].name` | string | Human-readable name of the value. | `"Milk"` |

</div>

> [!IMPORTANT]
> `code` must be unique.
>
> `appliesTo` supports only `material` and `product`.
>
> Classification values should use stable business codes because master-data records reference these codes directly.

See [Types and attributes](../master-data/types-and-attributes.md) for guidance on when to use controlled classifications instead of free-form attributes.

## Using types in master data

Types define the allowed values used by explicit classification fields on supported master-data resources.

For example, this Type:

```json
{
  "code": "allergen",
  "name": "Allergen",
  "appliesTo": "material",
  "values": [
    {
      "code": "ALG-MILK",
      "name": "Milk"
    }
  ]
}
```

allows a Material to reference the value by code:

```json
{
  "code": "MAT0001",
  "name": "Raw milk",
  "allergen": "ALG-MILK"
}
```

Supported classification fields currently include:

| Resource | Field |
| --- | --- |
| [Material](../master-data/material.md) | `allergen` |
| [Material](../master-data/material.md) | `storage` |
| [Material](../master-data/material.md) | `origin` |
| [Product](../master-data/product.md) | `packFormat` |

Types are controlled classifications. They are different from `attributes`, which are free-form source-system metadata and are not used as analytical classifications.

## Managing values

Submitting a Type does not replace its complete list of values.

Values included in the request are added when they do not exist or corrected when the same value code already exists. Values omitted from the request remain registered.

For example, submitting:

```json
{
  "code": "allergen",
  "name": "Allergen",
  "appliesTo": "material",
  "values": [
    {
      "code": "ALG-EGG",
      "name": "Egg"
    }
  ]
}
```

adds or corrects `ALG-EGG` without removing existing values such as `ALG-MILK`.

To remove a value explicitly, use the value-delete operation described below.

## API resource

| Resource | Base path |
| --- | --- |
| `Type` | `/services/pulse/food-beverage/types` |

## API methods

### Create or extend a type

`POST /services/pulse/food-beverage/types/insert`

Creates a Type or extends an existing Type.

If the Type already exists, its name and `appliesTo` value are updated and the supplied classification values are added or corrected. Existing values that are not included in the request are not removed.

#### Request

```http
POST /services/pulse/food-beverage/types/insert
Content-Type: application/json
```

```json
{
  "code": "allergen",
  "name": "Allergen",
  "appliesTo": "material",
  "values": [
    {
      "code": "ALG-MILK",
      "name": "Milk"
    },
    {
      "code": "ALG-NONE",
      "name": "None"
    }
  ]
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the Type. |
| `name` | string | yes | Human-readable name of the Type. |
| `appliesTo` | string | yes | Resource the Type applies to: `material` or `product`. |
| `values` | array | yes | Classification values to add or correct. |
| `values[].code` | string | yes | Business code of the value. |
| `values[].name` | string | yes | Human-readable name of the value. |

### Update a type

`PUT /services/pulse/food-beverage/types/update`

Updates an existing Type and adds or corrects the supplied values.

Values omitted from `values` are not removed.

#### Request

```http
PUT /services/pulse/food-beverage/types/update
Content-Type: application/json
```

```json
{
  "code": "allergen",
  "name": "Allergen classification",
  "appliesTo": "material",
  "values": [
    {
      "code": "ALG-MILK",
      "name": "Milk"
    },
    {
      "code": "ALG-EGG",
      "name": "Egg"
    }
  ]
}
```

### Patch a type

`PATCH /services/pulse/food-beverage/types/patch`

Partially updates an existing Type.

The Type is identified by `properties.code`. The supplied `values` are added or corrected; values not included in the patch remain unchanged.

#### Request

```http
PATCH /services/pulse/food-beverage/types/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "allergen",
    "name": "Allergen classification",
    "values": [
      {
        "code": "ALG-EGG",
        "name": "Egg"
      }
    ]
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Business code of the Type to update. |
| `properties.name` | string | no | New human-readable name. |
| `properties.appliesTo` | string | no | New target resource: `material` or `product`. |
| `properties.values` | array | no | Classification values to add or correct. |
| `properties.values[].code` | string | yes | Business code of the supplied value. |
| `properties.values[].name` | string | yes | Human-readable name of the supplied value. |

### Retrieve a type

`GET /services/pulse/food-beverage/types/select`

Returns the Type identified by its business code, including its registered values.

#### Request

```http
GET /services/pulse/food-beverage/types/select?id=allergen
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the Type to retrieve. |

#### Example response

```json
{
  "code": "allergen",
  "name": "Allergen",
  "appliesTo": "material",
  "values": [
    {
      "code": "ALG-MILK",
      "name": "Milk"
    },
    {
      "code": "ALG-NONE",
      "name": "None"
    }
  ]
}
```

### List types

`GET /services/pulse/food-beverage/types/query`

Returns Types matching the supplied filters, including their registered values.

#### Request

```http
GET /services/pulse/food-beverage/types/query?appliesTo=material
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `codes` | string or array of strings | no | Limits results to the specified Type business codes. |
| `names` | string or array of strings | no | Limits results to the specified names. |
| `appliesTo` | string or array of strings | no | Limits results to Types that apply to the specified resources. Supported values are `material` and `product`. |

#### Example response

```json
[
  {
    "code": "allergen",
    "name": "Allergen",
    "appliesTo": "material",
    "values": [
      {
        "code": "ALG-MILK",
        "name": "Milk"
      },
      {
        "code": "ALG-NONE",
        "name": "None"
      }
    ]
  }
]
```

### Remove a type value

`DELETE /services/pulse/food-beverage/types/values`

Removes a classification value from a Type.

#### Request

```http
DELETE /services/pulse/food-beverage/types/values?code=allergen&value=ALG-EGG
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Business code of the Type. |
| `value` | string | yes | Business code of the value to remove. |

### Delete a type

`DELETE /services/pulse/food-beverage/types/delete`

Deletes the Type identified by its business code.

#### Request

```http
DELETE /services/pulse/food-beverage/types/delete?id=allergen
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the Type to delete. |