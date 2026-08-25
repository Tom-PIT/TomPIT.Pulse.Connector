# Batch

<!-- TODO: This page is currently based on ApiSurfaceRevised. Revisit it once the implementation is available and verify fields, routes, query parameters, PATCH behavior, and examples against the current code. -->

Represents a bulk production batch such as a cook, fermentation, blend, or other vessel-based process step.

A batch produces a bulk lot that can later supply one or more production runs.

## The Batch object

```json
{
  "code": "BULK-260810-07",
  "vessel": "TANK-3",
  "recipe": "REC-BASE-v2",
  "start": "2026-08-10T18:00:00+02:00",
  "end": "2026-08-10T21:30:00+02:00",
  "quantity": 4200,
  "unit": "kg"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Unique business code used to identify the batch. The same code identifies the bulk lot produced by the batch. | `"BULK-260810-07"` |
| [`vessel`](../master-data/vessel.md) | string | Business code of the vessel in which the batch is processed. | `"TANK-3"` |
| [`recipe`](../master-data/recipe.md) | string or null | Optional business code of the recipe used for the batch. | `"REC-BASE-v2"` |
| `start` | string | Date and time when the batch started, in ISO 8601 format. | `"2026-08-10T18:00:00+02:00"` |
| `end` | string or null | Optional date and time when the batch finished. | `"2026-08-10T21:30:00+02:00"` |
| `quantity` | number or null | Optional quantity of bulk material produced. | `4200` |
| `unit` | string or null | Unit in which `quantity` is expressed. | `"kg"` |
| `expiresAt` | string or null | Optional expiry date or timestamp of the produced bulk lot. | `"2026-08-17"` |

</div>

> [!IMPORTANT]
> `code` must be unique. Two batches cannot use the same code.
>
> `vessel` must reference an existing Vessel.
>
> When `recipe` is provided, it must reference an existing Recipe.

## Produced bulk lot

Each batch produces a bulk lot using the same business code as the batch.

For example:

```text
Batch:    BULK-260810-07
Bulk lot: BULK-260810-07
```

That bulk lot can later be consumed by one or more production runs.

If `quantity` is provided, it represents the quantity produced by the batch.

## Batch and run

A batch and a [Run](run.md) represent different production activities.

```text
Process
  Batch
    ↓
  Bulk lot
    ↓
Packing
  Run
```

A batch represents bulk processing such as mixing, cooking, blending, or fermentation.

A run represents production of a product on a production line.

Keeping these activities separate preserves the connection between the process conditions that created the bulk material and the production activity that later used it.

## Expiry

`expiresAt` defines the expiry of the bulk lot produced by the batch.

When it is not provided, Pulse may derive the expiry from the shelf-life definition applicable to the product produced from the batch.

## API resource

| Resource | Base path |
| --- | --- |
| `Batch` | `/services/pulse/food-beverage/batches` |

## API methods

> [!NOTE]
> The API methods below follow the current Food & Beverage service pattern and are provisional until the Batches implementation is available for verification.

### Create a batch

`POST /services/pulse/food-beverage/batches/insert`

Creates a new production batch.

#### Request

```http
POST /services/pulse/food-beverage/batches/insert
Content-Type: application/json
```

```json
{
  "code": "BULK-260810-07",
  "vessel": "TANK-3",
  "recipe": "REC-BASE-v2",
  "start": "2026-08-10T18:00:00+02:00",
  "end": "2026-08-10T21:30:00+02:00",
  "quantity": 4200,
  "unit": "kg"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the batch. |
| `vessel` | string | yes | Business code of the vessel. |
| `recipe` | string or null | no | Business code of the recipe used. |
| `start` | string | yes | Date and time when the batch started. |
| `end` | string or null | no | Date and time when the batch finished. |
| `quantity` | number or null | no | Quantity produced by the batch. |
| `unit` | string or null | no | Unit of the produced quantity. |
| `expiresAt` | string or null | no | Expiry of the produced bulk lot. |


### Update a batch

`PUT /services/pulse/food-beverage/batches/update`

Updates an existing batch.

#### Request

```http
PUT /services/pulse/food-beverage/batches/update
Content-Type: application/json
```

```json
{
  "code": "BULK-260810-07",
  "vessel": "TANK-3",
  "recipe": "REC-BASE-v2",
  "start": "2026-08-10T18:00:00+02:00",
  "end": "2026-08-10T21:45:00+02:00",
  "quantity": 4200,
  "unit": "kg"
}
```


### Patch a batch

`PATCH /services/pulse/food-beverage/batches/patch`

Partially updates an existing batch.

The fields to update are supplied in the `properties` object. The batch is identified by its business `code`.

#### Request

```http
PATCH /services/pulse/food-beverage/batches/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "BULK-260810-07",
    "end": "2026-08-10T21:45:00+02:00",
    "quantity": 4200
  }
}
```


### Retrieve a batch

`GET /services/pulse/food-beverage/batches/select`

Returns the batch identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/batches/select?id=BULK-260810-07
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the batch to retrieve. |

#### Example response

```json
{
  "code": "BULK-260810-07",
  "vessel": "TANK-3",
  "recipe": "REC-BASE-v2",
  "start": "2026-08-10T18:00:00+02:00",
  "end": "2026-08-10T21:30:00+02:00",
  "quantity": 4200,
  "unit": "kg"
}
```


### List batches

`GET /services/pulse/food-beverage/batches/query`

Returns batches matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/batches/query?vessel=TANK-3&recipe=REC-BASE-v2
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `vessel` | string | no | Limits results to batches processed in the specified vessel. |
| `recipe` | string | no | Limits results to batches using the specified recipe. |
| `from` | string | no | Limits results to batches starting on or after the specified date or time. |
| `to` | string | no | Limits results to batches starting on or before the specified date or time. |


### Delete a batch

`DELETE /services/pulse/food-beverage/batches/delete`

Deletes the batch identified by its business code.

#### Request

```http
DELETE /services/pulse/food-beverage/batches/delete?id=BULK-260810-07
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the batch to delete. |