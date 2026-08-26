# Run

Represents a filling or packing production run for a Product on a Production line.

A Run is the main production activity used to connect production output, consumption, process measurements, line time, events, and stages.

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
| `code` | string | Unique business code used to identify the Run. | `"L01-260810-002"` |
| [`line`](../master-data/production-line.md) | string | Business code of the Production line on which the Run takes place. | `"LINE001"` |
| [`product`](../master-data/product.md) | string | Business code of the Product produced during the Run. | `"PRD001"` |
| [`recipe`](../master-data/recipe.md) | string or null | Optional business code of the Recipe used during the Run. | `"REC-PRD001-v3"` |
| [`shift`](../master-data/shift.md) | string or null | Optional business code of the Shift associated with the Run. | `"SHIFT_C"` |
| [`crew`](../master-data/crew.md) | string or null | Optional business code of the Crew assigned to the Run. | `"CREW-C"` |
| [`fromLots`](lot.md) | array of strings | Business codes of Lots consumed by the Run. Empty when no source Lots are specified. | `["BULK-260810-07"]` |
| `plannedQuantity` | number or null | Optional planned output quantity, expressed in the Product's unit. | `24000` |
| `start` | string | Date and time when the Run started, in ISO 8601 format. | `"2026-08-10T22:00:00+02:00"` |
| `end` | string or null | Date and time when the Run ended, or `null` while it remains open. | `"2026-08-11T06:00:00+02:00"` |
| `status` | string or null | Declared Run outcome. Supported values are `completed` and `cancelled`. | `"completed"` |

</div>

> [!IMPORTANT]
> `code` must be unique. Two Runs cannot use the same code.
>
> `line` and `product` must reference existing records.
>
> When `recipe`, `shift`, `crew`, or `fromLots` are provided, the referenced records must already exist.

## Run and batch

A Run and a [Batch](batch.md) represent different production activities.

```text
Bulk processing
  Batch
    ↓
  Bulk lot
    ↓
Filling / packing
  Run
```

A Batch produces bulk material.

A Run turns that material into a specific Product on a Production line.

The optional `fromLots` field preserves the connection between the Run and the Lots it consumed:

```json
{
  "code": "L01-260810-002",
  "fromLots": [
    "BULK-260810-07",
    "BULK-260810-08"
  ]
}
```

A Run can consume more than one Lot, and the same Lot can supply more than one Run.

## Planned quantity

`plannedQuantity` defines how much Product the Run was expected to produce.

The quantity is expressed in the unit defined by the referenced Product.

For example:

```json
{
  "product": "PRD001",
  "plannedQuantity": 24000
}
```

Providing a planned quantity allows actual production output to be compared with the original Run plan.

When `plannedQuantity` is removed, the corresponding output plan for the Run is removed as well.

## Run status

`status` describes the declared outcome of the Run.

A normally completed Run uses:

```json
{
  "status": "completed"
}
```

A Run that started but stopped before normal completion uses:

```json
{
  "status": "cancelled"
}
```

When no outcome has been declared, `status` is null.

> [!IMPORTANT]
> `cancelled` is different from deleting a Run.
>
> Use `cancelled` when production actually started but was stopped early. Delete a Run only when the Run record itself should not exist.

## Reference protection

A Production line, Product, Recipe, Shift, or Crew referenced by an existing Run cannot be deleted until the Run reference is removed.

## API resource

| Resource | Base path |
| --- | --- |
| `Run` | `/services/pulse/food-beverage/runs` |

## API methods

### Create a run

`POST /services/pulse/food-beverage/runs/insert`

Creates a new production Run.

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
| `code` | string | yes | Unique business code of the Run. |
| `line` | string | yes | Business code of the Production line. |
| `product` | string | yes | Business code of the Product. |
| `recipe` | string or null | no | Business code of the Recipe used. |
| `shift` | string or null | no | Business code of the Shift. |
| `crew` | string or null | no | Business code of the Crew. |
| `fromLots` | array of strings or null | no | Business codes of Lots consumed by the Run. |
| `plannedQuantity` | number or null | no | Planned output quantity in the Product's unit. |
| `start` | string | yes | Date and time when the Run started. |
| `end` | string or null | no | Date and time when the Run ended. |
| `status` | string or null | no | Declared outcome: `completed` or `cancelled`. |

### Update a run

`PUT /services/pulse/food-beverage/runs/update`

Updates an existing production Run.

The complete Run payload is submitted. Referenced resources and the original planned output are synchronized with the supplied values.

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

Partially updates an existing Run.

The Run is identified by `properties.code`. Fields omitted from `properties` keep their current values.

Optional fields can be explicitly cleared where applicable.

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

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Business code of the Run to update. |
| `properties.line` | string | no | New Production line business code. |
| `properties.product` | string | no | New Product business code. |
| `properties.recipe` | string or null | no | New Recipe business code, or `null` to clear it. |
| `properties.shift` | string or null | no | New Shift business code, or `null` to clear it. |
| `properties.crew` | string or null | no | New Crew business code, or `null` to clear it. |
| `properties.fromLots` | array of strings or null | no | Replacement set of consumed Lot codes. Use an empty array or `null` to remove all source Lots. |
| `properties.plannedQuantity` | number or null | no | New planned output quantity, or `null` to remove it. |
| `properties.start` | string | no | New Run start date and time. |
| `properties.end` | string or null | no | New Run end date and time, or `null` to clear it. |
| `properties.status` | string or null | no | New outcome, or `null` to clear the declared outcome. |

### Retrieve a run

`GET /services/pulse/food-beverage/runs/select`

Returns the Run identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/runs/select?id=L01-260810-002
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the Run to retrieve. |

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

Returns Runs matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/runs/query?lines=LINE001&products=PRD001
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `codes` | string or array of strings | no | Limits results to the specified Run business codes. |
| `lines` | string or array of strings | no | Limits results to Runs on the specified Production lines. |
| `products` | string or array of strings | no | Limits results to Runs producing the specified Products. |
| `shifts` | string or array of strings | no | Limits results to Runs associated with the specified Shifts. |
| `crews` | string or array of strings | no | Limits results to Runs associated with the specified Crews. |
| `from` | string | no | Limits results to Runs starting at or after the specified date and time. |
| `to` | string | no | Limits results to Runs starting at or before the specified date and time. |

Multiple values can be supplied by repeating the query parameter:

```http
GET /services/pulse/food-beverage/runs/query?lines=LINE001&lines=LINE002
```

### Delete a run

`DELETE /services/pulse/food-beverage/runs/delete`

Deletes the Run identified by its business code.

Use this only when the Run record should not exist. A Run that actually started but stopped early should use `status: "cancelled"` instead.

Deleting the Run also removes its associated original output plan and Run references.

#### Request

```http
DELETE /services/pulse/food-beverage/runs/delete?id=L01-260810-002
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the Run to delete. |