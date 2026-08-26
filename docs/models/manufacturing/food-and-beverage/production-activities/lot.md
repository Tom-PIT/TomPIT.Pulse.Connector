# Lot

Represents a traceable quantity of material received from a supplier.

A Lot records what arrived, when it arrived, how much was received, and optionally the composition or quality measurements reported for that delivery.

## The Lot object

```json
{
  "code": "IN-260807-441",
  "material": "MAT0001",
  "supplier": "SUP001",
  "receivedAt": "2026-08-07T05:20:00+02:00",
  "expiresAt": "2026-08-14T00:00:00+02:00",
  "quantity": 1000,
  "unit": "kg",
  "analysedAt": "2026-08-07T09:00:00+02:00",
  "analysis": {
    "fat": 3.82,
    "protein": 3.41,
    "ph": 6.68
  }
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Unique business code used to identify the Lot or delivery. | `"IN-260807-441"` |
| [`material`](../master-data/material.md) | string | Business code of the Material received. | `"MAT0001"` |
| [`supplier`](../master-data/supplier.md) | string | Business code of the Supplier that provided the Lot. | `"SUP001"` |
| `receivedAt` | string | Date and time when the Lot was received, in ISO 8601 format. | `"2026-08-07T05:20:00+02:00"` |
| `expiresAt` | string or null | Optional date and time when the Lot expires, in ISO 8601 format. | `"2026-08-14T00:00:00+02:00"` |
| `quantity` | number | Quantity received. | `1000` |
| `unit` | string | Unit in which `quantity` is expressed. | `"kg"` |
| `analysedAt` | string or null | Date and time when the Lot analysis was performed. | `"2026-08-07T09:00:00+02:00"` |
| `analysis` | object | Measured composition or quality values keyed by Measurement business code. | `{ "fat": 3.82, "protein": 3.41 }` |

</div>

> [!IMPORTANT]
> `code` must be unique. Two Lots cannot use the same code.
>
> `material` and `supplier` must reference existing records.
>
> Every key in `analysis` must reference an existing [Measurement](../definitions-and-rules/measurement.md).
>
> When `analysis` contains one or more values, `analysedAt` is required.

## Lot analysis

Analysis values describe measured properties of the received Lot, such as fat, protein, pH, moisture, or Brix.

Each key in `analysis` is the business code of a declared [Measurement](../definitions-and-rules/measurement.md):

```json
{
  "analysis": {
    "fat": 3.82,
    "protein": 3.41,
    "ph": 6.68
  }
}
```

The analysis belongs to the Lot rather than to a later production activity.

This allows Pulse to preserve properties measured for the specific incoming material as that Lot is subsequently consumed in production.

### Replacing analysis

Submitting `analysis` through an update replaces the existing analysis values for the Lot.

For example, if the existing analysis contains:

```json
{
  "fat": 3.82,
  "protein": 3.41
}
```

and the Lot is updated with:

```json
{
  "analysis": {
    "fat": 3.85
  }
}
```

the resulting analysis contains only the newly submitted `fat` value.

An empty or null analysis removes the existing analysis values.

## Traceability

A Lot distinguishes one received quantity of Material from another.

For example, two deliveries of the same Material may come from different Suppliers or have different measured properties:

```text
MAT0001
├── IN-260807-441
└── IN-260808-112
```

Production records can then reference the specific Lot that was consumed, preserving traceability from incoming Material through subsequent production activity.

A Material or Supplier that is referenced by an existing Lot cannot be deleted until the reference is removed.

## API resource

| Resource | Base path |
| --- | --- |
| `Lot` | `/services/pulse/food-beverage/lots` |

## API methods

### Create a lot

`POST /services/pulse/food-beverage/lots/insert`

Creates a new incoming Lot.

#### Request

```http
POST /services/pulse/food-beverage/lots/insert
Content-Type: application/json
```

```json
{
  "code": "IN-260807-441",
  "material": "MAT0001",
  "supplier": "SUP001",
  "receivedAt": "2026-08-07T05:20:00+02:00",
  "expiresAt": "2026-08-14T00:00:00+02:00",
  "quantity": 1000,
  "unit": "kg",
  "analysedAt": "2026-08-07T09:00:00+02:00",
  "analysis": {
    "fat": 3.82,
    "protein": 3.41,
    "ph": 6.68
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the Lot. |
| `material` | string | yes | Business code of the received Material. |
| `supplier` | string | yes | Business code of the Supplier. |
| `receivedAt` | string | yes | Date and time when the Lot was received. |
| `expiresAt` | string or null | no | Date and time when the Lot expires. |
| `quantity` | number | yes | Quantity received. |
| `unit` | string | yes | Unit of the received quantity. |
| `analysedAt` | string or null | conditional | Required when `analysis` contains one or more values. |
| `analysis` | object or null | no | Measured values keyed by Measurement business code. |

### Update a lot

`PUT /services/pulse/food-beverage/lots/update`

Updates an existing Lot.

The complete Lot payload is submitted. Analysis values supplied in the request replace the existing analysis for the Lot.

#### Request

```http
PUT /services/pulse/food-beverage/lots/update
Content-Type: application/json
```

```json
{
  "code": "IN-260807-441",
  "material": "MAT0001",
  "supplier": "SUP001",
  "receivedAt": "2026-08-07T05:20:00+02:00",
  "expiresAt": "2026-08-15T00:00:00+02:00",
  "quantity": 1000,
  "unit": "kg",
  "analysedAt": "2026-08-07T09:00:00+02:00",
  "analysis": {
    "fat": 3.82,
    "protein": 3.41,
    "ph": 6.68
  }
}
```

### Patch a lot

`PATCH /services/pulse/food-beverage/lots/patch`

Partially updates an existing Lot.

The Lot is identified by `properties.code`. Fields omitted from `properties` keep their current values.

`expiresAt`, `analysedAt`, and `analysis` can be explicitly cleared by including them with a null value.

#### Request

```http
PATCH /services/pulse/food-beverage/lots/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "IN-260807-441",
    "expiresAt": "2026-08-15T00:00:00+02:00"
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Business code of the Lot to update. |
| `properties.material` | string | no | New Material business code. |
| `properties.supplier` | string | no | New Supplier business code. |
| `properties.receivedAt` | string | no | New receipt date and time. |
| `properties.expiresAt` | string or null | no | New expiry date and time, or `null` to clear it. |
| `properties.quantity` | number | no | New received quantity. |
| `properties.unit` | string | no | New quantity unit. |
| `properties.analysedAt` | string or null | no | New analysis timestamp, or `null` to clear it. |
| `properties.analysis` | object or null | no | Replacement analysis values, or `null` to remove the existing analysis. |

### Retrieve a lot

`GET /services/pulse/food-beverage/lots/select`

Returns the Lot identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/lots/select?id=IN-260807-441
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the Lot to retrieve. |

#### Example response

```json
{
  "code": "IN-260807-441",
  "material": "MAT0001",
  "supplier": "SUP001",
  "receivedAt": "2026-08-07T05:20:00+02:00",
  "expiresAt": "2026-08-14T00:00:00+02:00",
  "quantity": 1000,
  "unit": "kg",
  "analysedAt": "2026-08-07T09:00:00+02:00",
  "analysis": {
    "fat": 3.82,
    "protein": 3.41,
    "ph": 6.68
  }
}
```

### List lots

`GET /services/pulse/food-beverage/lots/query`

Returns Lots matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/lots/query?materials=MAT0001&suppliers=SUP001
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `codes` | string or array of strings | no | Limits results to the specified Lot business codes. |
| `materials` | string or array of strings | no | Limits results to Lots of the specified Materials. |
| `suppliers` | string or array of strings | no | Limits results to Lots received from the specified Suppliers. |
| `receivedFrom` | string | no | Limits results to Lots received at or after the specified date and time. |
| `receivedTo` | string | no | Limits results to Lots received at or before the specified date and time. |

Multiple values can be supplied by repeating the query parameter:

```http
GET /services/pulse/food-beverage/lots/query?materials=MAT0001&materials=MAT0002
```

### Delete a lot

`DELETE /services/pulse/food-beverage/lots/delete`

Deletes the Lot identified by its business code.

Deleting the Lot also removes the receipt and analysis records associated with it.

#### Request

```http
DELETE /services/pulse/food-beverage/lots/delete?id=IN-260807-441
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the Lot to delete. |