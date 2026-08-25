# Customer

Represents a customer associated with Food & Beverage operations in Pulse.

## The Customer object

```json
{
  "code": "CUST001",
  "name": "Example Retail Slovenia",
  "taxNumber": "SI12345678",
  "group": "Retail"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Unique business code used to identify the customer in external systems and integrations. | `"CUST001"` |
| `name` | string | Registered or human-readable name of the customer. | `"Example Retail Slovenia"` |
| `taxNumber` | string or null | Tax number used to identify the customer across source systems. | `"SI12345678"` |
| `group` | string or null | Optional customer grouping used by the source system. | `"Retail"` |

</div>

> [!IMPORTANT]
> `code` must be unique. Two customers cannot use the same code.

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## API resource

| Resource | Base path |
| --- | --- |
| `Customer` | `/services/pulse/food-beverage/customers` |

## API methods

### Create a customer

`POST /services/pulse/food-beverage/customers/insert`

Creates a new customer.

#### Request

```http
POST /services/pulse/food-beverage/customers/insert
Content-Type: application/json
```

```json
{
  "code": "CUST001",
  "name": "Example Retail Slovenia",
  "taxNumber": "SI12345678",
  "group": "Retail"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the customer. |
| `name` | string | yes | Registered or human-readable name of the customer. |
| `taxNumber` | string or null | no | Tax number used to identify the customer across source systems. |
| `group` | string or null | no | Optional customer grouping used by the source system. |


### Update a customer

`PUT /services/pulse/food-beverage/customers/update`

Updates an existing customer.

#### Request

```http
PUT /services/pulse/food-beverage/customers/update
Content-Type: application/json
```

```json
{
  "code": "CUST001",
  "name": "Example Retail Slovenia",
  "taxNumber": "SI12345678",
  "group": "Retail"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the customer to update. |
| `name` | string | yes | Registered or human-readable name of the customer. |
| `taxNumber` | string or null | no | Tax number used to identify the customer across source systems. |
| `group` | string or null | no | Optional customer grouping used by the source system. |


### Patch a customer

`PATCH /services/pulse/food-beverage/customers/patch`

Partially updates an existing customer.

The fields to update are supplied in the `properties` object. The customer is identified by its business `code`.

#### Request

```http
PATCH /services/pulse/food-beverage/customers/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "CUST001",
    "group": "Key account"
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Unique business code of the customer to update. |
| `properties.name` | string | no | New registered or human-readable name of the customer. |
| `properties.taxNumber` | string or null | no | New tax number. |
| `properties.group` | string or null | no | New customer grouping. |


### Retrieve a customer

`GET /services/pulse/food-beverage/customers/select`

Returns the customer identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/customers/select?id=CUST001
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the customer to retrieve. |

#### Example response

```json
{
  "code": "CUST001",
  "name": "Example Retail Slovenia",
  "taxNumber": "SI12345678",
  "group": "Retail"
}
```


### List customers

`GET /services/pulse/food-beverage/customers/query`

Returns customers matching the supplied filters.

Customers can be filtered by code, name, tax number, and group.

#### Request

```http
GET /services/pulse/food-beverage/customers/query?groups=Retail
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `codes` | string or array of strings | no | Limits results to customers with the specified business codes. |
| `names` | string or array of strings | no | Limits results to customers with the specified names. |
| `taxNumbers` | string or array of strings | no | Limits results to customers with the specified tax numbers. |
| `groups` | string or array of strings | no | Limits results to customers in the specified groups. |

#### Example response

```json
[
  {
    "code": "CUST001",
    "name": "Example Retail Slovenia",
    "taxNumber": "SI12345678",
    "group": "Retail"
  }
]
```


### Delete a customer

`DELETE /services/pulse/food-beverage/customers/delete`

Deletes the customer identified by its business code.

#### Request

```http
DELETE /services/pulse/food-beverage/customers/delete?id=CUST001
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the customer to delete. |