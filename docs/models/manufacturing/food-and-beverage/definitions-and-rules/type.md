# Type

<!-- TODO: This page is currently based on ApiSurfaceRevised. Revisit it once the implementation is available and verify fields, routes, query parameters, PATCH behavior, and examples against the current code. -->

Defines a controlled classification that can be applied to a Material or Product.

Types are used for classifications such as allergens, storage conditions, origin, or pack format. Each type defines a set of allowed values that can then be referenced from supported master-data fields.

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
| `code` | string | Unique business code used to identify the type. | `"allergen"` |
| `name` | string | Human-readable name of the type. | `"Allergen"` |
| `appliesTo` | string | Resource the classification applies to. Supported values are `material` and `product`. | `"material"` |
| `values` | array | Allowed classification values. At least one value is required. | |
| `values[].code` | string | Unique business code of the value within the type. | `"ALG-MILK"` |
| `values[].name` | string | Human-readable name of the value. | `"Milk"` |

</div>

> [!IMPORTANT]
> `code` must be unique.
>
> Classification values should use stable business codes because master-data records reference these codes directly.

See [Types and attributes](../master-data/types-and-attributes.md) for guidance on when to use controlled classifications instead of free-form attributes.

## Using types in master data

Types define the allowed values used by explicit classification fields on supported master-data resources.

For example, this type:

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

## Type size

Keep classification types relatively small and meaningful.

The current model guidance recommends keeping a type below approximately 25 distinct values. Very high-cardinality classifications are too granular to be useful as analytical dimensions.

For example, use a broader origin grouping rather than creating one value for every individual farm or supplier location.

## API resource

| Resource | Base path |
| --- | --- |
| `Type` | `/services/pulse/food-beverage/types` |

## API methods

> [!NOTE]
> The API methods below are based on ApiSurfaceRevised and are provisional until the Types implementation is available for verification.

### Create or extend a type

`POST /services/pulse/food-beverage/types/insert`

Creates a type and its allowed values.

The current specification also describes repeated submissions as additive: new values are added and existing names may be corrected without removing previously registered values.

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
| `code` | string | yes | Unique business code of the type. |
| `name` | string | yes | Human-readable name of the type. |
| `appliesTo` | string | yes | Resource the type applies to: `material` or `product`. |
| `values` | array | yes | Classification values. At least one value is required. |
| `values[].code` | string | yes | Business code of the value. |
| `values[].name` | string | yes | Human-readable name of the value. |

### Retrieve a type

`GET /services/pulse/food-beverage/types/select`

Returns the type identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/types/select?id=allergen
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the type to retrieve. |

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

Returns types matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/types/query?appliesTo=material
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `codes` | string or array of strings | no | Limits results to the specified type codes. |
| `names` | string or array of strings | no | Limits results to the specified names. |
| `appliesTo` | string | no | Limits results to types that apply to `material` or `product`. |

### Remove a type value

The specification defines removal of individual values separately from the type itself.

A value that is already referenced by master data cannot be removed.

The exact route and request shape should be verified once the implementation is available.