# Hold

Represents finished stock placed on hold for a quality decision.

A Hold records which Lot was withheld, why it was held, how much stock was affected, and optionally the decision and recovered value when the Hold is resolved.

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
| `code` | string | Unique business code used to identify the Hold. | `"HOLD-2211"` |
| [`lot`](lot.md) | string | Business code of the Lot placed on hold. | `"FG-260810-113"` |
| [`product`](../master-data/product.md) | string or null | Optional business code of the Product associated with the held Lot. | `"PRD001"` |
| [`reason`](../definitions-and-rules/reason.md) | string | Business code of the Reason for the Hold. | `"DTC03"` |
| `quantity` | number | Quantity affected by the Hold. | `4200` |
| `unit` | string | Unit in which the held quantity is expressed. | `"pcs"` |
| `start` | string | Date and time when the Hold started, in ISO 8601 format. | `"2026-08-11T07:15:00+02:00"` |
| `end` | string or null | Date and time when the Hold ended, or `null` while it remains open. | `"2026-08-12T11:00:00+02:00"` |
| `decision` | string or null | Optional Hold outcome. Supported values are `released`, `downgraded`, and `scrapped`. | `"downgraded"` |
| `recoveredUnitValue` | number or null | Optional value recovered per unit when the held stock retains commercial value. | `0.41` |

</div>

> [!IMPORTANT]
> `code` must be unique. Two Holds cannot use the same code.
>
> `lot` and `reason` must reference existing records.
>
> When `product` is provided, it must reference an existing Product.

## Hold lifecycle

A Hold remains open while `end` is null:

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

Supplying `end` closes the Hold:

```json
{
  "end": "2026-08-12T11:00:00+02:00",
  "decision": "downgraded",
  "recoveredUnitValue": 0.41
}
```

```text
end = null    → open
end supplied  → closed
```

The time between `start` and `end` represents how long the stock remained on hold.

## Decision

A Hold can record one of these decisions:

| Decision | Meaning |
| --- | --- |
| `released` | The held stock was approved for its intended use. |
| `downgraded` | The stock was retained for a lower-value use. |
| `scrapped` | The held stock was rejected and scrapped. |

`decision` is optional. Closing a Hold does not require a decision value to be supplied.

## Recovered value

`recoveredUnitValue` records the unit value retained when held stock keeps some commercial value.

For example:

```json
{
  "decision": "downgraded",
  "recoveredUnitValue": 0.41
}
```

The recovered value is associated with the resolution of the Hold. It is retained when the Hold has an `end` timestamp.

This allows Pulse to distinguish partial loss from complete loss without recalculating historical value from a later price list.

## Why hold duration matters

A Hold is represented as an activity with a beginning and an end rather than as a single event.

While stock is held, it is unavailable and quality work may be required to reach a decision.

Recording `start` and `end` allows Hold duration to be compared across Products, Reasons, and other production context.

## Reference protection

A Lot, Product, or Reason referenced by an existing Hold cannot be deleted until the Hold reference is removed.

## API resource

| Resource | Base path |
| --- | --- |
| `Hold` | `/services/pulse/food-beverage/holds` |

## API methods

### Create a hold

`POST /services/pulse/food-beverage/holds/insert`

Creates a new quality Hold.

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
| `code` | string | yes | Unique business code of the Hold. |
| `lot` | string | yes | Business code of the held Lot. |
| `product` | string or null | no | Business code of the associated Product. |
| `reason` | string | yes | Business code of the Reason for the Hold. |
| `quantity` | number | yes | Quantity placed on hold. |
| `unit` | string | yes | Unit of the held quantity. |
| `start` | string | yes | Date and time when the Hold started. |
| `end` | string or null | no | Date and time when the Hold ended. |
| `decision` | string or null | no | `released`, `downgraded`, or `scrapped`. |
| `recoveredUnitValue` | number or null | no | Recovered value per unit. |

### Update a hold

`PUT /services/pulse/food-beverage/holds/update`

Updates an existing quality Hold.

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

Partially updates an existing Hold.

The Hold is identified by `properties.code`. Fields omitted from `properties` keep their current values.

`product`, `end`, `decision`, and `recoveredUnitValue` can be explicitly cleared by including them with a null value.

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

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Business code of the Hold to update. |
| `properties.lot` | string | no | New Lot business code. |
| `properties.product` | string or null | no | New Product business code, or `null` to clear it. |
| `properties.reason` | string | no | New Reason business code. |
| `properties.quantity` | number | no | New held quantity. |
| `properties.unit` | string | no | New quantity unit. |
| `properties.start` | string | no | New Hold start date and time. |
| `properties.end` | string or null | no | New Hold end date and time, or `null` to reopen it. |
| `properties.decision` | string or null | no | New decision, or `null` to clear it. |
| `properties.recoveredUnitValue` | number or null | no | New recovered value per unit, or `null` to clear it. |

### Retrieve a hold

`GET /services/pulse/food-beverage/holds/select`

Returns the Hold identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/holds/select?id=HOLD-2211
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the Hold to retrieve. |

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

Returns Holds matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/holds/query?products=PRD001&decisions=downgraded
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `codes` | string or array of strings | no | Limits results to the specified Hold business codes. |
| `products` | string or array of strings | no | Limits results to Holds associated with the specified Products. |
| `decisions` | string or array of strings | no | Limits results to the specified decisions. |
| `open` | boolean | no | `true` returns open Holds; `false` returns closed Holds. |

Multiple values can be supplied by repeating the query parameter:

```http
GET /services/pulse/food-beverage/holds/query?decisions=downgraded&decisions=scrapped
```

### Delete a hold

`DELETE /services/pulse/food-beverage/holds/delete`

Deletes the Hold identified by its business code.

Use this only when the Hold record itself should not exist.

#### Request

```http
DELETE /services/pulse/food-beverage/holds/delete?id=HOLD-2211
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the Hold to delete. |