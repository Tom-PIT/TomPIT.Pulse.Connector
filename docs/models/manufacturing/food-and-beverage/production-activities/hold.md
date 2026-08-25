# Hold

<!-- TODO: This page is currently based on ApiSurfaceRevised. Revisit it once the implementation is available and verify fields, routes, query parameters, PATCH behavior, and examples against the current code. -->

Represents a quality hold placed on a finished lot.

A hold records which lot was withheld, why it was held, how much stock was affected, and what was decided when the hold was resolved.

## The Hold object

```json
{
  "code": "HOLD-2211",
  "lot": "FG-260810-113",
  "product": "PRD001",
  "reason": "DTC03",
  "quantity": 4200,
  "unit": "pcs",
  "start": "2026-08-11T07:15:00+02:00",
  "end": "2026-08-12T11:00:00+02:00",
  "decision": "downgraded",
  "recoveredUnitValue": 0.41
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Unique business code used to identify the hold. | `"HOLD-2211"` |
| [`lot`](lot.md) | string | Business code of the finished lot placed on hold. | `"FG-260810-113"` |
| [`product`](../master-data/product.md) | string or null | Optional business code of the product associated with the held lot. | `"PRD001"` |
| [`reason`](../definitions-and-rules/reason.md) | string | Business code of the reason for the hold. | `"DTC03"` |
| `quantity` | number | Quantity affected by the hold. | `4200` |
| `unit` | string | Unit in which the held quantity is expressed. | `"pcs"` |
| `start` | string | Date and time when the hold started, in ISO 8601 format. | `"2026-08-11T07:15:00+02:00"` |
| `end` | string or null | Optional date and time when the hold was resolved. Omit while the hold is still open. | `"2026-08-12T11:00:00+02:00"` |
| `decision` | string or null | Outcome of the hold. Supported values are `released`, `downgraded`, and `scrapped`. | `"downgraded"` |
| `recoveredUnitValue` | number or null | Optional unit value recovered when the held product retains commercial value. | `0.41` |

</div>

> [!IMPORTANT]
> `code` must be unique. Two holds cannot use the same code.
>
> `lot` and `reason` must reference existing records.
>
> When `product` is provided, it must reference an existing Product.

## Hold lifecycle

A hold remains open while `end` is not provided.

For example:

```json
{
  "code": "HOLD-2211",
  "lot": "FG-260810-113",
  "product": "PRD001",
  "reason": "DTC03",
  "quantity": 4200,
  "unit": "pcs",
  "start": "2026-08-11T07:15:00+02:00"
}
```

When the investigation is complete, close the hold with `end` and the resulting `decision`:

```json
{
  "code": "HOLD-2211",
  "end": "2026-08-12T11:00:00+02:00",
  "decision": "downgraded",
  "recoveredUnitValue": 0.41
}
```

The time between `start` and `end` represents how long the finished stock remained on hold.

## Decision

A resolved hold can have one of these decisions:

| Decision | Meaning |
| --- | --- |
| `released` | The held stock is released for normal use or sale. |
| `downgraded` | The stock is retained but at a lower grade or value. |
| `scrapped` | The held stock is discarded. |

A hold with no decision is still unresolved.

## Recovered value

`recoveredUnitValue` records the actual unit value retained when held stock is downgraded or otherwise retains some commercial value.

For example:

```json
{
  "decision": "downgraded",
  "recoveredUnitValue": 0.41
}
```

The value should reflect what the unit was worth at the time the decision was made.

This allows Pulse to distinguish partial loss from complete loss without recalculating historical value from a later price list.

## Why hold duration matters

A hold is treated as an activity with a beginning and an end rather than as a single event.

While stock is held, it is unavailable and quality work is required to reach a decision.

Recording `start` and `end` allows hold duration to be compared across products, reasons, and other production context.

## API resource

| Resource | Base path |
| --- | --- |
| `Hold` | `/services/pulse/food-beverage/holds` |

## API methods

> [!NOTE]
> The API methods below follow the current Food & Beverage service pattern and are provisional until the Holds implementation is available for verification.

### Create a hold

`POST /services/pulse/food-beverage/holds/insert`

Creates a new quality hold.

#### Request

```http
POST /services/pulse/food-beverage/holds/insert
Content-Type: application/json
```

```json
{
  "code": "HOLD-2211",
  "lot": "FG-260810-113",
  "product": "PRD001",
  "reason": "DTC03",
  "quantity": 4200,
  "unit": "pcs",
  "start": "2026-08-11T07:15:00+02:00"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the hold. |
| `lot` | string | yes | Business code of the finished lot. |
| `product` | string or null | no | Business code of the associated product. |
| `reason` | string | yes | Business code of the reason for the hold. |
| `quantity` | number | yes | Quantity placed on hold. |
| `unit` | string | yes | Unit of the held quantity. |
| `start` | string | yes | Date and time when the hold started. |
| `end` | string or null | no | Date and time when the hold was resolved. |
| `decision` | string or null | no | `released`, `downgraded`, or `scrapped`. |
| `recoveredUnitValue` | number or null | no | Recovered value per unit. |


### Update a hold

`PUT /services/pulse/food-beverage/holds/update`

Updates an existing quality hold.

#### Request

```http
PUT /services/pulse/food-beverage/holds/update
Content-Type: application/json
```

```json
{
  "code": "HOLD-2211",
  "lot": "FG-260810-113",
  "product": "PRD001",
  "reason": "DTC03",
  "quantity": 4200,
  "unit": "pcs",
  "start": "2026-08-11T07:15:00+02:00",
  "end": "2026-08-12T11:00:00+02:00",
  "decision": "downgraded",
  "recoveredUnitValue": 0.41
}
```


### Patch a hold

`PATCH /services/pulse/food-beverage/holds/patch`

Partially updates an existing hold.

The fields to update are supplied in the `properties` object. The hold is identified by its business `code`.

#### Request

```http
PATCH /services/pulse/food-beverage/holds/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "HOLD-2211",
    "end": "2026-08-12T11:00:00+02:00",
    "decision": "downgraded",
    "recoveredUnitValue": 0.41
  }
}
```


### Retrieve a hold

`GET /services/pulse/food-beverage/holds/select`

Returns the hold identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/holds/select?id=HOLD-2211
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the hold to retrieve. |

#### Example response

```json
{
  "code": "HOLD-2211",
  "lot": "FG-260810-113",
  "product": "PRD001",
  "reason": "DTC03",
  "quantity": 4200,
  "unit": "pcs",
  "start": "2026-08-11T07:15:00+02:00",
  "end": "2026-08-12T11:00:00+02:00",
  "decision": "downgraded",
  "recoveredUnitValue": 0.41
}
```


### List holds

`GET /services/pulse/food-beverage/holds/query`

Returns holds matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/holds/query?product=PRD001&decision=downgraded
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `product` | string | no | Limits results to holds for the specified product. |
| `decision` | string | no | Limits results to holds with the specified decision. |
| `open` | boolean | no | When `true`, returns holds that have not yet been resolved. |


### Delete a hold

`DELETE /services/pulse/food-beverage/holds/delete`

Deletes the hold identified by its business code.

#### Request

```http
DELETE /services/pulse/food-beverage/holds/delete?id=HOLD-2211
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the hold to delete. |