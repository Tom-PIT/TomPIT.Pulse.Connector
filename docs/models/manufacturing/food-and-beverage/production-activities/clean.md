# Clean

Represents a cleaning activity performed on a Production line between two Products.

A Clean records which cleaning regime was used, the Product transition it applied to, and how long the cleaning activity took.

## The Clean object

```json
{
  "code": "CIP-260811-014",
  "line": "LINE001",
  "regime": "allergen-cip",
  "productBefore": "PRD001",
  "productAfter": "PRD044",
  "start": "2026-08-11T06:00:00+02:00",
  "end": "2026-08-11T07:50:00+02:00"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Unique business code used to identify the Clean. | `"CIP-260811-014"` |
| [`line`](../master-data/production-line.md) | string | Business code of the Production line being cleaned. | `"LINE001"` |
| [`regime`](../definitions-and-rules/clean-regime.md) | string | Business code of the Clean regime used. | `"allergen-cip"` |
| [`productBefore`](../master-data/product.md) | string | Business code of the Product produced before the Clean. | `"PRD001"` |
| [`productAfter`](../master-data/product.md) | string | Business code of the Product produced after the Clean. | `"PRD044"` |
| `start` | string | Date and time when the Clean started, in ISO 8601 format. | `"2026-08-11T06:00:00+02:00"` |
| `end` | string or null | Date and time when the Clean ended, or `null` while it remains open. | `"2026-08-11T07:50:00+02:00"` |

</div>

> [!IMPORTANT]
> `code` must be unique. Two Cleans cannot use the same code.
>
> `line`, `regime`, `productBefore`, and `productAfter` must reference existing records.

## Product transition

The Product pair defines the changeover that the Clean belongs to.

```text
PRD001
   │
   │ clean
   ▼
PRD044
```

This is important because cleaning requirements can depend on the direction of the Product transition.

For example:

```text
PRD001 → PRD044
```

may require a different regime or expected duration than:

```text
PRD044 → PRD001
```

The actual Clean can therefore be compared with the corresponding [Cleaning rule](../definitions-and-rules/cleaning-rule.md).

## Clean status

A Clean without `end` remains open.

When `end` is supplied, the Clean is treated as completed.

```text
end = null    → running
end supplied  → completed
```

## Cleaning duration

The duration of the cleaning activity is determined from `start` and `end`.

For example:

```json
{
  "start": "2026-08-11T06:00:00+02:00",
  "end": "2026-08-11T07:50:00+02:00"
}
```

records a Clean that took 110 minutes.

This actual duration can be compared with `expectedMinutes` from the applicable Cleaning rule.

## Planned and actual resource use

Cleaning can consume chemicals, water, energy, Labor, equipment time, and other resources.

Expected resource use can be recorded through [Planned use](planned-use.md).

Actual resource use is recorded separately through [Consumption](../operational-data/consumption.md).

This keeps the cleaning activity itself separate from the resources consumed while performing it.

## Reference protection

A Production line, Clean regime, or Product referenced by an existing Clean cannot be deleted until the Clean reference is removed.

## API resource

| Resource | Base path |
| --- | --- |
| `Clean` | `/services/pulse/food-beverage/cleans` |

## API methods

### Create a clean

`POST /services/pulse/food-beverage/cleans/insert`

Creates a new cleaning activity.

#### Request

```http
POST /services/pulse/food-beverage/cleans/insert
Content-Type: application/json
```

```json
{
  "code": "CIP-260811-014",
  "line": "LINE001",
  "regime": "allergen-cip",
  "productBefore": "PRD001",
  "productAfter": "PRD044",
  "start": "2026-08-11T06:00:00+02:00",
  "end": "2026-08-11T07:50:00+02:00"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the Clean. |
| `line` | string | yes | Business code of the Production line. |
| `regime` | string | yes | Business code of the Clean regime. |
| `productBefore` | string | yes | Business code of the Product produced before the Clean. |
| `productAfter` | string | yes | Business code of the Product produced after the Clean. |
| `start` | string | yes | Date and time when the Clean started. |
| `end` | string or null | no | Date and time when the Clean ended. |

### Update a clean

`PUT /services/pulse/food-beverage/cleans/update`

Updates an existing cleaning activity.

#### Request

```http
PUT /services/pulse/food-beverage/cleans/update
Content-Type: application/json
```

```json
{
  "code": "CIP-260811-014",
  "line": "LINE001",
  "regime": "allergen-cip",
  "productBefore": "PRD001",
  "productAfter": "PRD044",
  "start": "2026-08-11T06:00:00+02:00",
  "end": "2026-08-11T07:55:00+02:00"
}
```

### Patch a clean

`PATCH /services/pulse/food-beverage/cleans/patch`

Partially updates an existing Clean.

The Clean is identified by `properties.code`. Fields omitted from `properties` keep their current values.

`end` can be explicitly cleared by including it with a null value.

#### Request

```http
PATCH /services/pulse/food-beverage/cleans/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "CIP-260811-014",
    "end": "2026-08-11T07:55:00+02:00"
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Business code of the Clean to update. |
| `properties.line` | string | no | New Production line business code. |
| `properties.regime` | string | no | New Clean regime business code. |
| `properties.productBefore` | string | no | New preceding Product business code. |
| `properties.productAfter` | string | no | New following Product business code. |
| `properties.start` | string | no | New Clean start date and time. |
| `properties.end` | string or null | no | New Clean end date and time, or `null` to leave the Clean open. |

### Retrieve a clean

`GET /services/pulse/food-beverage/cleans/select`

Returns the Clean identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/cleans/select?id=CIP-260811-014
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the Clean to retrieve. |

#### Example response

```json
{
  "code": "CIP-260811-014",
  "line": "LINE001",
  "regime": "allergen-cip",
  "productBefore": "PRD001",
  "productAfter": "PRD044",
  "start": "2026-08-11T06:00:00+02:00",
  "end": "2026-08-11T07:50:00+02:00"
}
```

### List cleans

`GET /services/pulse/food-beverage/cleans/query`

Returns Cleans matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/cleans/query?lines=LINE001&regimes=allergen-cip
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `codes` | string or array of strings | no | Limits results to the specified Clean business codes. |
| `lines` | string or array of strings | no | Limits results to Cleans on the specified Production lines. |
| `regimes` | string or array of strings | no | Limits results to the specified Clean regimes. |
| `from` | string | no | Limits results to Cleans starting at or after the specified date and time. |
| `to` | string | no | Limits results to Cleans starting at or before the specified date and time. |

Multiple values can be supplied by repeating the query parameter:

```http
GET /services/pulse/food-beverage/cleans/query?lines=LINE001&lines=LINE002
```

### Delete a clean

`DELETE /services/pulse/food-beverage/cleans/delete`

Deletes the Clean identified by its business code.

Use delete only when the cleaning activity should not exist as a recorded activity.

#### Request

```http
DELETE /services/pulse/food-beverage/cleans/delete?id=CIP-260811-014
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the Clean to delete. |