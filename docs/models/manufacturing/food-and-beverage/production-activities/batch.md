# Batch

Represents a bulk production batch such as a cook, fermentation, blend, or other vessel-based process step.

A Batch produces a bulk Lot that can later supply one or more production runs.

## The Batch object

```json
{
  "code": "BULK-260810-07",
  "vessel": "TANK-3",
  "recipe": "REC-BASE-v2",
  "start": "2026-08-10T18:00:00+02:00",
  "end": "2026-08-10T21:30:00+02:00",
  "quantity": 4200,
  "unit": "kg",
  "expiresAt": "2026-08-17T21:30:00+02:00"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Unique business code used to identify the Batch. The same code identifies the bulk Lot produced by the Batch. | `"BULK-260810-07"` |
| [`vessel`](../master-data/vessel.md) | string | Business code of the Vessel in which the Batch is processed. | `"TANK-3"` |
| [`recipe`](../master-data/recipe.md) | string or null | Optional business code of the Recipe used by the Batch. | `"REC-BASE-v2"` |
| `start` | string | Date and time when the Batch started, in ISO 8601 format. | `"2026-08-10T18:00:00+02:00"` |
| `end` | string or null | Date and time when the Batch finished, or `null` while it is still running. | `"2026-08-10T21:30:00+02:00"` |
| `quantity` | number or null | Optional quantity of bulk material produced. | `4200` |
| `unit` | string or null | Unit of the produced quantity. Returned from the configured `good-quantity` Measurement when a quantity exists. | `"kg"` |
| `expiresAt` | string or null | Expiry date and time of the produced bulk Lot. May be supplied explicitly or derived from shelf life. | `"2026-08-17T21:30:00+02:00"` |

</div>

> [!IMPORTANT]
> `code` must be unique. Two Batches cannot use the same code.
>
> `vessel` must reference an existing [Vessel](../master-data/vessel.md).
>
> When `recipe` is provided, it must reference an existing [Recipe](../master-data/recipe.md).
>
> `end` is required when `quantity` is supplied.

## Produced bulk lot

Each Batch produces a bulk Lot using the same business code as the Batch.

For example:

```text
Batch:    BULK-260810-07
Bulk lot: BULK-260810-07
```

The bulk Lot is created when the Batch is created and updated together with the Batch.

Its production timestamp is:

- `end` when the Batch is complete;
- otherwise `start`.

The bulk Lot can later be consumed by one or more production Runs.

Deleting the Batch also deletes its produced bulk Lot.

## Batch status

A Batch without `end` is treated as running.

When `end` is supplied, the Batch is treated as completed.

```text
end = null    → running
end supplied  → completed
```

A produced `quantity` can only be recorded for a completed Batch.

## Batch and run

A Batch and a [Run](run.md) represent different production activities.

```text
Process
  Batch
    ↓
  Bulk lot
    ↓
Packing
  Run
```

A Batch represents bulk processing such as mixing, cooking, blending, or fermentation.

A Run represents production of a Product on a Production line.

Keeping these activities separate preserves the connection between the process conditions that created the bulk material and the production activity that later used it.

## Expiry

`expiresAt` defines the expiry of the bulk Lot produced by the Batch.

When `expiresAt` is supplied explicitly, Pulse uses that value.

When it is omitted, Pulse can derive the expiry when the Batch references a Recipe whose parent Product has an effective Product limit for the `shelf-life-days` Measurement.

The effective shelf-life limit is evaluated at:

- `end` when the Batch is complete;
- otherwise `start`.

Pulse then calculates:

```text
expiresAt = effective timestamp + shelf-life-days target
```

If no applicable shelf-life Target exists, `expiresAt` remains null.

## Reference protection

A Recipe or Vessel referenced by an existing Batch cannot be deleted until the Batch reference is removed.

## API resource

| Resource | Base path |
| --- | --- |
| `Batch` | `/services/pulse/food-beverage/batches` |

## API methods

### Create a batch

`POST /services/pulse/food-beverage/batches/insert`

Creates a new Batch and its produced bulk Lot.

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
  "unit": "kg",
  "expiresAt": "2026-08-17T21:30:00+02:00"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the Batch. |
| `vessel` | string | yes | Business code of the Vessel. |
| `recipe` | string or null | no | Business code of the Recipe used. |
| `start` | string | yes | Date and time when the Batch started. |
| `end` | string or null | conditional | Date and time when the Batch finished. Required when `quantity` is supplied. |
| `quantity` | number or null | no | Quantity produced by the Batch. |
| `unit` | string or null | no | Unit associated with the produced quantity. |
| `expiresAt` | string or null | no | Expiry of the produced bulk Lot. When omitted, Pulse may derive it from shelf life. |

### Update a batch

`PUT /services/pulse/food-beverage/batches/update`

Updates an existing Batch and synchronizes its produced bulk Lot.

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
  "unit": "kg",
  "expiresAt": null
}
```

### Patch a batch

`PATCH /services/pulse/food-beverage/batches/patch`

Partially updates an existing Batch.

The Batch is identified by `properties.code`. Fields omitted from `properties` keep their current values.

`recipe`, `end`, `quantity`, `unit`, and `expiresAt` can be explicitly cleared by including them with a null value.

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

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Business code of the Batch to update. |
| `properties.vessel` | string | no | New Vessel business code. |
| `properties.recipe` | string or null | no | New Recipe business code, or `null` to clear it. |
| `properties.start` | string | no | New Batch start date and time. |
| `properties.end` | string or null | no | New Batch end date and time, or `null` to mark it incomplete. |
| `properties.quantity` | number or null | no | New produced quantity, or `null` to clear it. |
| `properties.unit` | string or null | no | New quantity unit, or `null` to clear it. |
| `properties.expiresAt` | string or null | no | New expiry date and time, or `null` to allow expiry to be derived when possible. |

### Retrieve a batch

`GET /services/pulse/food-beverage/batches/select`

Returns the Batch identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/batches/select?id=BULK-260810-07
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the Batch to retrieve. |

#### Example response

```json
{
  "code": "BULK-260810-07",
  "vessel": "TANK-3",
  "recipe": "REC-BASE-v2",
  "start": "2026-08-10T18:00:00+02:00",
  "end": "2026-08-10T21:30:00+02:00",
  "quantity": 4200,
  "unit": "kg",
  "expiresAt": "2026-08-17T21:30:00+02:00"
}
```

### List batches

`GET /services/pulse/food-beverage/batches/query`

Returns Batches matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/batches/query?vessels=TANK-3&recipes=REC-BASE-v2
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `codes` | string or array of strings | no | Limits results to the specified Batch business codes. |
| `vessels` | string or array of strings | no | Limits results to Batches processed in the specified Vessels. |
| `recipes` | string or array of strings | no | Limits results to Batches using the specified Recipes. |
| `from` | string | no | Limits results to Batches starting at or after the specified date and time. |
| `to` | string | no | Limits results to Batches starting at or before the specified date and time. |

Multiple values can be supplied by repeating the query parameter:

```http
GET /services/pulse/food-beverage/batches/query?vessels=TANK-3&vessels=TANK-4
```

### Delete a batch

`DELETE /services/pulse/food-beverage/batches/delete`

Deletes the Batch identified by its business code.

Deleting a Batch also deletes its produced bulk Lot.

#### Request

```http
DELETE /services/pulse/food-beverage/batches/delete?id=BULK-260810-07
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the Batch to delete. |