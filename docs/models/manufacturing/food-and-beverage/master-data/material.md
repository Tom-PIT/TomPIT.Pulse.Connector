# Material

Represents an ingredient, packaging material, chemical, utility, or other material consumed in Food & Beverage operations.

A material can optionally belong to a parent material used as a material group.

## The Material object

```json
{
  "code": "MAT0001",
  "name": "Raw milk 3.8% fat",
  "unit": "kg",
  "parent": "MG-DAIRY-RAW",
  "lotTracked": true,
  "allergen": "ALG-MILK",
  "storage": "CHILLED",
  "origin": "SI"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Unique business code used to identify the material in external systems and integrations. | `"MAT0001"` |
| `name` | string | Human-readable name of the material. | `"Raw milk 3.8% fat"` |
| `unit` | string | Unit used to measure quantities of the material. | `"kg"` |
| `parent` | string or null | Business code of the parent material used as the material group. | `"MG-DAIRY-RAW"` |
| `lotTracked` | boolean | Indicates whether consumption of the material must reference a lot. | `true` |
| `allergen` | string or null | Allergen type-value code associated with the material. | `"ALG-MILK"` |
| `storage` | string or null | Storage type-value code associated with the material. | `"CHILLED"` |
| `origin` | string or null | Country-of-origin type-value code associated with the material. | `"SI"` |

</div>

> [!IMPORTANT]
> `code` must be unique. Two materials cannot use the same code.
>
> When `parent` is provided, the referenced parent material must already exist.
>
> When `allergen`, `storage`, or `origin` are provided, the referenced type values must already exist.

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## Lot tracking

Materials that require traceability should use `lotTracked: true`.

When `lotTracked` is `true`, consumption records for the material must reference a [lot](../production-activities/lot.md).

Materials that are not normally lot-tracked, such as water, steam, electricity, or similar utilities, can use `lotTracked: false`.

## API resource

| Resource | Base path |
| --- | --- |
| `Material` | `/services/pulse/food-beverage/materials` |

## API methods

### Create a material

`POST /services/pulse/food-beverage/materials/insert`

Creates a new material.

#### Request

```http
POST /services/pulse/food-beverage/materials/insert
Content-Type: application/json
```

```json
{
  "code": "MAT0001",
  "name": "Raw milk 3.8% fat",
  "unit": "kg",
  "parent": "MG-DAIRY-RAW",
  "lotTracked": true,
  "allergen": "ALG-MILK",
  "storage": "CHILLED",
  "origin": "SI"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the material. |
| `name` | string | yes | Human-readable name of the material. |
| `unit` | string | yes | Unit used to measure quantities of the material. |
| `parent` | string or null | no | Business code of the parent material used as the material group. |
| `lotTracked` | boolean | yes | Indicates whether consumption of the material must reference a lot. |
| `allergen` | string or null | no | Allergen type-value code associated with the material. |
| `storage` | string or null | no | Storage type-value code associated with the material. |
| `origin` | string or null | no | Country-of-origin type-value code associated with the material. |


### Update a material

`PUT /services/pulse/food-beverage/materials/update`

Updates an existing material.

#### Request

```http
PUT /services/pulse/food-beverage/materials/update
Content-Type: application/json
```

```json
{
  "code": "MAT0001",
  "name": "Raw milk 3.8% fat",
  "unit": "kg",
  "parent": "MG-DAIRY-RAW",
  "lotTracked": true,
  "allergen": "ALG-MILK",
  "storage": "CHILLED",
  "origin": "SI"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the material to update. |
| `name` | string | yes | Human-readable name of the material. |
| `unit` | string | yes | Unit used to measure quantities of the material. |
| `parent` | string or null | no | Business code of the parent material used as the material group. |
| `lotTracked` | boolean | yes | Indicates whether consumption of the material must reference a lot. |
| `allergen` | string or null | no | Allergen type-value code associated with the material. |
| `storage` | string or null | no | Storage type-value code associated with the material. |
| `origin` | string or null | no | Country-of-origin type-value code associated with the material. |


### Patch a material

`PATCH /services/pulse/food-beverage/materials/patch`

Partially updates an existing material.

The fields to update are supplied in the `properties` object. The material is identified by its business `code`.

> [!NOTE]
> The exact PATCH implementation for Material still needs to be confirmed against `Materials/Ops/Patch.cs`.

#### Request

```http
PATCH /services/pulse/food-beverage/materials/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "MAT0001",
    "storage": "FROZEN",
    "lotTracked": true
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Unique business code of the material to update. |
| `properties.name` | string | no | New human-readable name of the material. |
| `properties.unit` | string | no | New unit used to measure quantities of the material. |
| `properties.parent` | string or null | no | New parent material used as the material group. |
| `properties.lotTracked` | boolean | no | New lot-tracking setting. |
| `properties.allergen` | string or null | no | New allergen type-value code. |
| `properties.storage` | string or null | no | New storage type-value code. |
| `properties.origin` | string or null | no | New country-of-origin type-value code. |


### Retrieve a material

`GET /services/pulse/food-beverage/materials/select`

Returns the material identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/materials/select?id=MAT0001
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the material to retrieve. |

#### Example response

```json
{
  "code": "MAT0001",
  "name": "Raw milk 3.8% fat",
  "unit": "kg",
  "parent": "MG-DAIRY-RAW",
  "lotTracked": true,
  "allergen": "ALG-MILK",
  "storage": "CHILLED",
  "origin": "SI"
}
```


### List materials

`GET /services/pulse/food-beverage/materials/query`

Returns materials matching the supplied filters.

Materials can be filtered by code, name, parent group, lot-tracking status, allergen, storage, and origin.

#### Request

```http
GET /services/pulse/food-beverage/materials/query?parents=MG-DAIRY-RAW&lotTracked=true&allergens=ALG-MILK
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `codes` | string or array of strings | no | Limits results to materials with the specified business codes. |
| `names` | string or array of strings | no | Limits results to materials with the specified names. |
| `parents` | string or array of strings | no | Limits results to materials belonging to the specified parent groups. |
| `lotTracked` | boolean | no | Limits results by lot-tracking status. |
| `allergens` | string or array of strings | no | Limits results to materials with the specified allergen classifications. |
| `storages` | string or array of strings | no | Limits results to materials with the specified storage classifications. |
| `origins` | string or array of strings | no | Limits results to materials with the specified origin classifications. |

#### Example response

```json
[
  {
    "code": "MAT0001",
    "name": "Raw milk 3.8% fat",
    "unit": "kg",
    "parent": "MG-DAIRY-RAW",
    "lotTracked": true,
    "allergen": "ALG-MILK",
    "storage": "CHILLED",
    "origin": "SI"
  }
]
```


### Delete a material

`DELETE /services/pulse/food-beverage/materials/delete`

Deletes the material identified by its business code.

#### Request

```http
DELETE /services/pulse/food-beverage/materials/delete?id=MAT0001
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the material to delete. |