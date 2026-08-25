# Lot

<!-- TODO: This page is currently based on ApiSurfaceRevised. Revisit it once the implementation is available and verify fields, routes, query parameters, PATCH behavior, and examples against the current code. -->

Represents a traceable quantity of material received from a supplier.

A lot records what arrived, when it arrived, how much was received, and optionally the composition or quality measurements reported for that delivery.

## The Lot object

```json
{
  "code": "IN-260807-441",
  "material": "MAT0001",
  "supplier": "SUP001",
  "receivedAt": "2026-08-07T05:20:00+02:00",
  "expiresAt": "2026-08-14",
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
| `code` | string | Unique business code used to identify the lot or delivery. | `"IN-260807-441"` |
| [`material`](../master-data/material.md) | string | Business code of the material received. | `"MAT0001"` |
| [`supplier`](../master-data/supplier.md) | string | Business code of the supplier that provided the lot. | `"SUP001"` |
| `receivedAt` | string | Date and time when the lot was received, in ISO 8601 format. | `"2026-08-07T05:20:00+02:00"` |
| `expiresAt` | string or null | Optional expiry date or timestamp of the lot. | `"2026-08-14"` |
| `quantity` | number | Quantity received. | `1000` |
| `unit` | string | Unit in which `quantity` is expressed. | `"kg"` |
| `analysedAt` | string or null | Date and time when the lot analysis was performed. Required when `analysis` is provided. | `"2026-08-07T09:00:00+02:00"` |
| `analysis` | object or null | Optional measured composition or quality values for the lot, keyed by measurement code. | `{ "fat": 3.82, "protein": 3.41 }` |

</div>

> [!IMPORTANT]
> `code` must be unique. Two lots cannot use the same code.
>
> `material` and `supplier` must reference existing records.
>
> When `analysis` contains one or more values, `analysedAt` is required.

## Lot analysis

Analysis values describe measured properties of the received lot, such as fat, protein, pH, moisture, or Brix.

Each key in `analysis` references a declared [Measurement](../definitions-and-rules/measurements.md):

```json
{
  "analysis": {
    "fat": 3.82,
    "protein": 3.41,
    "ph": 6.68
  }
}
```

The analysis belongs to the lot rather than to a later production run.

This allows Pulse to preserve the properties of the specific incoming material as that lot is consumed in production.

## Traceability

A lot distinguishes one received quantity of material from another.

For example, two deliveries of the same material may come from different suppliers or have different composition values:

```text
MAT0001
├── IN-260807-441
└── IN-260808-112
```

Production records can then reference the specific lot that was consumed, preserving traceability from incoming material through subsequent production activity.

## API resource

| Resource | Base path |
| --- | --- |
| `Lot` | `/services/pulse/food-beverage/lots` |

## API methods

> [!NOTE]
> The API methods below follow the current Food & Beverage service pattern and are provisional until the Lots implementation is available for verification.

### Create a lot

`POST /services/pulse/food-beverage/lots/insert`

Creates a new incoming lot.

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
  "expiresAt": "2026-08-14",
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
| `code` | string | yes | Unique business code of the lot. |
| `material` | string | yes | Business code of the received material. |
| `supplier` | string | yes | Business code of the supplier. |
| `receivedAt` | string | yes | Date and time when the lot was received. |
| `expiresAt` | string or null | no | Expiry date or timestamp of the lot. |
| `quantity` | number | yes | Quantity received. |
| `unit` | string | yes | Unit of the received quantity. |
| `analysedAt` | string or null | conditional | Required when `analysis` is provided. |
| `analysis` | object or null | no | Measured values keyed by measurement code. |


### Update a lot

`PUT /services/pulse/food-beverage/lots/update`

Updates an existing lot.

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
  "expiresAt": "2026-08-15",
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

Partially updates an existing lot.

The fields to update are supplied in the `properties` object. The lot is identified by its business `code`.

#### Request

```http
PATCH /services/pulse/food-beverage/lots/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "IN-260807-441",
    "expiresAt": "2026-08-15"
  }
}
```


### Retrieve a lot

`GET /services/pulse/food-beverage/lots/select`

Returns the lot identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/lots/select?id=IN-260807-441
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the lot to retrieve. |

#### Example response

```json
{
  "code": "IN-260807-441",
  "material": "MAT0001",
  "supplier": "SUP001",
  "receivedAt": "2026-08-07T05:20:00+02:00",
  "expiresAt": "2026-08-14",
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

Returns lots matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/lots/query?material=MAT0001&supplier=SUP001
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `material` | string | no | Limits results to lots of the specified material. |
| `supplier` | string | no | Limits results to lots received from the specified supplier. |
| `receivedFrom` | string | no | Limits results to lots received on or after the specified date or time. |
| `receivedTo` | string | no | Limits results to lots received on or before the specified date or time. |


### Delete a lot

`DELETE /services/pulse/food-beverage/lots/delete`

Deletes the lot identified by its business code.

#### Request

```http
DELETE /services/pulse/food-beverage/lots/delete?id=IN-260807-441
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the lot to delete. |