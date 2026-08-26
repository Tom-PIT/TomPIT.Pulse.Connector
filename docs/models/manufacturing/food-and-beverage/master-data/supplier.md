# Supplier

Represents a supplier associated with materials and other inputs used in Food & Beverage operations.

## The Supplier object

```json
{
  "code": "SUP001",
  "name": "Alpine Milk Cooperative Slovenia",
  "taxNumber": "SI12345678",
  "group": "Cooperative"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Unique business code used to identify the supplier in external systems and integrations. | `"SUP001"` |
| `name` | string | Registered or human-readable name of the supplier. | `"Alpine Milk Cooperative Slovenia"` |
| `taxNumber` | string or null | Tax number used to identify the supplier across source systems. | `"SI12345678"` |
| `group` | string or null | Optional supplier grouping used by the source system. | `"Cooperative"` |

</div>

> [!IMPORTANT]
> `code` must be unique. Two suppliers cannot use the same code.

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## API resource

| Resource | Base path |
| --- | --- |
| `Supplier` | `/services/pulse/food-beverage/suppliers` |

## API methods

### Create a supplier

`POST /services/pulse/food-beverage/suppliers/insert`

Creates a new supplier.

#### Request

```http
POST /services/pulse/food-beverage/suppliers/insert
Content-Type: application/json
```

```json
{
  "code": "SUP001",
  "name": "Alpine Milk Cooperative Slovenia",
  "taxNumber": "SI12345678",
  "group": "Cooperative"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the supplier. |
| `name` | string | yes | Registered or human-readable name of the supplier. |
| `taxNumber` | string or null | no | Tax number used to identify the supplier across source systems. |
| `group` | string or null | no | Optional supplier grouping used by the source system. |


### Update a supplier

`PUT /services/pulse/food-beverage/suppliers/update`

Updates an existing supplier.

#### Request

```http
PUT /services/pulse/food-beverage/suppliers/update
Content-Type: application/json
```

```json
{
  "code": "SUP001",
  "name": "Alpine Milk Cooperative Slovenia",
  "taxNumber": "SI12345678",
  "group": "Cooperative"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the supplier to update. |
| `name` | string | yes | Registered or human-readable name of the supplier. |
| `taxNumber` | string or null | no | Tax number used to identify the supplier across source systems. |
| `group` | string or null | no | Optional supplier grouping used by the source system. |


### Patch a supplier

`PATCH /services/pulse/food-beverage/suppliers/patch`

Partially updates an existing supplier.

The fields to update are supplied in the `properties` object. The supplier is identified by its business `code`.

#### Request

```http
PATCH /services/pulse/food-beverage/suppliers/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "SUP001",
    "group": "Dairy cooperative"
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Unique business code of the supplier to update. |
| `properties.name` | string | no | New registered or human-readable name of the supplier. |
| `properties.taxNumber` | string or null | no | New tax number. |
| `properties.group` | string or null | no | New supplier grouping. |


### Retrieve a supplier

`GET /services/pulse/food-beverage/suppliers/select`

Returns the supplier identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/suppliers/select?id=SUP001
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the supplier to retrieve. |

#### Example response

```json
{
  "code": "SUP001",
  "name": "Alpine Milk Cooperative Slovenia",
  "taxNumber": "SI12345678",
  "group": "Cooperative"
}
```


### List suppliers

`GET /services/pulse/food-beverage/suppliers/query`

Returns suppliers matching the supplied filters.

Suppliers can be filtered by code, name, tax number, and group.

#### Request

```http
GET /services/pulse/food-beverage/suppliers/query?groups=Cooperative
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `codes` | string or array of strings | no | Limits results to suppliers with the specified business codes. |
| `names` | string or array of strings | no | Limits results to suppliers with the specified names. |
| `taxNumbers` | string or array of strings | no | Limits results to suppliers with the specified tax numbers. |
| `groups` | string or array of strings | no | Limits results to suppliers in the specified groups. |

#### Example response

```json
[
  {
    "code": "SUP001",
    "name": "Alpine Milk Cooperative Slovenia",
    "taxNumber": "SI12345678",
    "group": "Cooperative"
  }
]
```


### Delete a supplier

`DELETE /services/pulse/food-beverage/suppliers/delete`

Deletes the supplier identified by its business code.

#### Request

```http
DELETE /services/pulse/food-beverage/suppliers/delete?id=SUP001
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the supplier to delete. |