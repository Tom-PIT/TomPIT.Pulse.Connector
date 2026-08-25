# Product

Represents a finished product or other output tracked in Pulse.

A product can optionally belong to a parent product representing its brand.

## The Product object

```json
{
  "code": "PRD001",
  "name": "Yogurt Strawberry 125g",
  "parent": "BRAND-A",
  "unit": "pcs",
  "unitPrice": 1.34,
  "packFormat": "CUP"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Unique business code used to identify the product in external systems and integrations. | `"PRD001"` |
| `name` | string | Human-readable name of the product. | `"Yogurt Strawberry 125g"` |
| `parent` | string or null | Business code of the parent product representing the brand. | `"BRAND-A"` |
| `unit` | string | Unit used to count or measure the product. | `"pcs"` |
| `unitPrice` | number or null | Contractual selling price per product unit. | `1.34` |
| `packFormat` | string or null | Pack-format value code declared through Types. | `"CUP"` |

</div>

> [!IMPORTANT]
> `code` must be unique. Two products cannot use the same code.
>
> When `parent` is provided, the referenced parent product must already exist.
>
> When `packFormat` is provided, the referenced type value must already exist.

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## API resource

| Resource | Base path |
| --- | --- |
| `Product` | `/services/pulse/food-beverage/products` |

## API methods

### Create a product

`POST /services/pulse/food-beverage/products/insert`

Creates a new product.

#### Request

```http
POST /services/pulse/food-beverage/products/insert
Content-Type: application/json
```

```json
{
  "code": "PRD001",
  "name": "Yogurt Strawberry 125g",
  "parent": "BRAND-A",
  "unit": "pcs",
  "unitPrice": 1.34,
  "packFormat": "CUP"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the product. |
| `name` | string | yes | Human-readable name of the product. |
| `parent` | string or null | no | Business code of the parent product representing the brand. |
| `unit` | string | yes | Unit used to count or measure the product. |
| `unitPrice` | number or null | no | Contractual selling price per product unit. |
| `packFormat` | string or null | no | Pack-format value code declared through Types. |


### Update a product

`PUT /services/pulse/food-beverage/products/update`

Updates an existing product.

#### Request

```http
PUT /services/pulse/food-beverage/products/update
Content-Type: application/json
```

```json
{
  "code": "PRD001",
  "name": "Yogurt Strawberry 125g",
  "parent": "BRAND-A",
  "unit": "pcs",
  "unitPrice": 1.39,
  "packFormat": "CUP"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the product to update. |
| `name` | string | yes | Human-readable name of the product. |
| `parent` | string or null | no | Business code of the parent product representing the brand. |
| `unit` | string | yes | Unit used to count or measure the product. |
| `unitPrice` | number or null | no | Contractual selling price per product unit. |
| `packFormat` | string or null | no | Pack-format value code declared through Types. |


### Patch a product

`PATCH /services/pulse/food-beverage/products/patch`

Partially updates an existing product.

The fields to update are supplied in the `properties` object. The product is identified by its business `code`.

#### Request

```http
PATCH /services/pulse/food-beverage/products/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "PRD001",
    "unitPrice": 1.39,
    "packFormat": "CUP"
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Unique business code of the product to update. |
| `properties.name` | string | no | New human-readable name of the product. |
| `properties.parent` | string or null | no | New parent product representing the brand. |
| `properties.unit` | string | no | New unit used to count or measure the product. |
| `properties.unitPrice` | number or null | no | New contractual selling price per product unit. |
| `properties.packFormat` | string or null | no | New pack-format value code. |


### Retrieve a product

`GET /services/pulse/food-beverage/products/select`

Returns the product identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/products/select?id=PRD001
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the product to retrieve. |

#### Example response

```json
{
  "code": "PRD001",
  "name": "Yogurt Strawberry 125g",
  "parent": "BRAND-A",
  "unit": "pcs",
  "unitPrice": 1.34,
  "packFormat": "CUP"
}
```


### List products

`GET /services/pulse/food-beverage/products/query`

Returns products matching the supplied filters.

Products can be filtered by code, name, parent brand, and pack format.

#### Request

```http
GET /services/pulse/food-beverage/products/query?parents=BRAND-A&packFormats=CUP
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `codes` | string or array of strings | no | Limits results to products with the specified business codes. |
| `names` | string or array of strings | no | Limits results to products with the specified names. |
| `parents` | string or array of strings | no | Limits results to products with the specified parent brands. |
| `packFormats` | string or array of strings | no | Limits results to products with the specified pack formats. |

#### Example response

```json
[
  {
    "code": "PRD001",
    "name": "Yogurt Strawberry 125g",
    "parent": "BRAND-A",
    "unit": "pcs",
    "unitPrice": 1.34,
    "packFormat": "CUP"
  },
  {
    "code": "PRD002",
    "name": "Yogurt Vanilla 125g",
    "parent": "BRAND-A",
    "unit": "pcs",
    "unitPrice": 1.29,
    "packFormat": "CUP"
  }
]
```


### Delete a product

`DELETE /services/pulse/food-beverage/products/delete`

Deletes the product identified by its business code.

#### Request

```http
DELETE /services/pulse/food-beverage/products/delete?id=PRD001
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the product to delete. |