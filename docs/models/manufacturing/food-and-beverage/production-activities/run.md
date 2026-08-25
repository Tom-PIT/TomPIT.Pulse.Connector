# Run

<!-- TODO: This page is currently based on ApiSurfaceRevised. Revisit it once the implementation is available and verify fields, routes, query parameters, PATCH behavior, and examples against the current code. -->

Represents a filling or packing production run for a product on a production line.

A run is the main unit used to connect production output, consumption, process measurements, line states, events, and stages.

## The Run object

```json
{
  "code": "L01-260810-002",
  "line": "LINE001",
  "product": "PRD001",
  "recipe": "REC-PRD001-v3",
  "shift": "SHIFT_C",
  "crew": "CREW-C",
  "fromLots": [
    "BULK-260810-07"
  ],
  "plannedQuantity": 24000,
  "start": "2026-08-10T22:00:00+02:00",
  "end": "2026-08-11T06:00:00+02:00",
  "status": "completed"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Unique business code used to identify the run. | `"L01-260810-002"` |
| [`line`](../master-data/production-line.md) | string | Business code of the production line on which the run takes place. | `"LINE001"` |
| [`product`](../master-data/product.md) | string | Business code of the product produced during the run. | `"PRD001"` |
| [`recipe`](../master-data/recipe.md) | string or null | Optional business code of the recipe used during the run. | `"REC-PRD001-v3"` |
| [`shift`](../master-data/shift.md) | string or null | Optional business code of the shift associated with the run. | `"SHIFT_C"` |
| [`crew`](../master-data/crew.md) | string or null | Optional business code of the crew associated with the run. | `"CREW-C"` |
| [`fromLots`](lot.md) | array of strings or null | Optional business codes of bulk lots consumed by the run. | `["BULK-260810-07"]` |
| `plannedQuantity` | number or null | Optional planned output quantity, expressed in the product's own unit. | `24000` |
| `start` | string | Date and time when the run started, in ISO 8601 format. | `"2026-08-10T22:00:00+02:00"` |
| `end` | string or null | Optional date and time when the run ended. | `"2026-08-11T06:00:00+02:00"` |
| `status` | string or null | How the run ended. Supported values are `completed` and `cancelled`. | `"completed"` |

</div>

> [!IMPORTANT]
> `code` must be unique. Two runs cannot use the same code.
>
> `line` and `product` must reference existing records.
>
> When `recipe`, `shift`, `crew`, or `fromLots` are provided, the referenced records must already exist.

## Run and batch

A run and a [Batch](batch.md) represent different production activities.

```text
Bulk processing
  Batch
    ↓
  Bulk lot
    ↓
Filling / packing
  Run
```

A batch produces bulk material.

A run turns that bulk material into a specific product on a production line.

The optional `fromLots` field preserves the connection between the run and the bulk lots it consumed.

```json
{
  "code": "L01-260810-002",
  "fromLots": [
    "BULK-260810-07",
    "BULK-260810-08"
  ]
}
```

A run can consume more than one bulk lot, and one bulk lot can supply more than one run.

## Planned quantity

`plannedQuantity` defines how much product the run was expected to produce.

The quantity is expressed in the unit defined by the Product.

For example:

```json
{
  "product": "PRD001",
  "plannedQuantity": 24000
}
```

Providing the planned quantity allows actual production output to be compared with what the run was intended to produce.

Without a planned quantity, Pulse can still record output, but it cannot determine how much production was lost relative to the run plan.

## Run status

A completed run uses:

```json
{
  "status": "completed"
}
```

A run that started but was stopped before normal completion uses:

```json
{
  "status": "cancelled"
}
```

> [!IMPORTANT]
> `cancelled` is different from deleting a run.
>
> Use `cancelled` when production actually started but was stopped early. Delete a run only when the run record itself should not exist.

## API resource

| Resource | Base path |
| --- | --- |
| `Run` | `/services/pulse/food-beverage/runs` |

## API methods

> [!NOTE]
> The API methods below follow the current Food & Beverage service pattern and are provisional until the Runs implementation is available for verification.

### Create a run

`POST /services/pulse/food-beverage/runs/insert`

Creates a new production run.

#### Request

```http
POST /services/pulse/food-beverage/runs/insert
Content-Type: application/json
```

```json
{
  "code": "L01-260810-002",
  "line": "LINE001",
  "product": "PRD001",
  "recipe": "REC-PRD001-v3",
  "shift": "SHIFT_C",
  "crew": "CREW-C",
  "fromLots": [
    "BULK-260810-07"
  ],
  "plannedQuantity": 24000,
  "start": "2026-08-10T22:00:00+02:00",
  "end": "2026-08-11T06:00:00+02:00",
  "status": "completed"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the run. |
| `line` | string | yes | Business code of the production line. |
| `product` | string | yes | Business code of the product. |
| `recipe` | string or null | no | Business code of the recipe used. |
| `shift` | string or null | no | Business code of the shift. |
| `crew` | string or null | no | Business code of the crew. |
| `fromLots` | array of strings or null | no | Bulk lot codes consumed by the run. |
| `plannedQuantity` | number or null | no | Planned output quantity in the product's unit. |
| `start` | string | yes | Date and time when the run started. |
| `end` | string or null | no | Date and time when the run ended. |
| `status` | string or null | no | `completed` or `cancelled`. |


### Update a run

`PUT /services/pulse/food-beverage/runs/update`

Updates an existing production run.

#### Request

```http
PUT /services/pulse/food-beverage/runs/update
Content-Type: application/json
```

```json
{
  "code": "L01-260810-002",
  "line": "LINE001",
  "product": "PRD001",
  "recipe": "REC-PRD001-v3",
  "shift": "SHIFT_C",
  "crew": "CREW-C",
  "fromLots": [
    "BULK-260810-07"
  ],
  "plannedQuantity": 24000,
  "start": "2026-08-10T22:00:00+02:00",
  "end": "2026-08-11T06:15:00+02:00",
  "status": "completed"
}
```


### Patch a run

`PATCH /services/pulse/food-beverage/runs/patch`

Partially updates an existing run.

The fields to update are supplied in the `properties` object. The run is identified by its business `code`.

#### Request

```http
PATCH /services/pulse/food-beverage/runs/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "L01-260810-002",
    "end": "2026-08-11T06:15:00+02:00",
    "status": "completed"
  }
}
```


### Retrieve a run

`GET /services/pulse/food-beverage/runs/select`

Returns the run identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/runs/select?id=L01-260810-002
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the run to retrieve. |

#### Example response

```json
{
  "code": "L01-260810-002",
  "line": "LINE001",
  "product": "PRD001",
  "recipe": "REC-PRD001-v3",
  "shift": "SHIFT_C",
  "crew": "CREW-C",
  "fromLots": [
    "BULK-260810-07"
  ],
  "plannedQuantity": 24000,
  "start": "2026-08-10T22:00:00+02:00",
  "end": "2026-08-11T06:00:00+02:00",
  "status": "completed"
}
```


### List runs

`GET /services/pulse/food-beverage/runs/query`

Returns runs matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/runs/query?line=LINE001&product=PRD001
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `line` | string | no | Limits results to runs on the specified production line. |
| `product` | string | no | Limits results to runs producing the specified product. |
| `shift` | string | no | Limits results to runs associated with the specified shift. |
| `crew` | string | no | Limits results to runs associated with the specified crew. |
| `from` | string | no | Limits results to runs starting on or after the specified date or time. |
| `to` | string | no | Limits results to runs starting on or before the specified date or time. |


### Delete a run

`DELETE /services/pulse/food-beverage/runs/delete`

Deletes the run identified by its business code.

Use this only when the run record should not exist. A run that actually started but stopped early should use `status: "cancelled"` instead.

#### Request

```http
DELETE /services/pulse/food-beverage/runs/delete?id=L01-260810-002
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the run to delete. |