# Clean

<!-- TODO: This page is currently based on ApiSurfaceRevised. Revisit it once the implementation is available and verify fields, routes, query parameters, PATCH behavior, and examples against the current code. -->

Represents a cleaning activity performed on a production line between two products.

A clean records which cleaning regime was used, the product transition it applied to, and how long the cleaning activity took.

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
| `code` | string | Unique business code used to identify the cleaning activity. | `"CIP-260811-014"` |
| [`line`](../master-data/production-line.md) | string | Business code of the production line being cleaned. | `"LINE001"` |
| [`regime`](../definitions-and-rules/clean-regime.md) | string | Business code of the cleaning regime used. | `"allergen-cip"` |
| [`productBefore`](../master-data/product.md) | string | Business code of the product produced before the clean. | `"PRD001"` |
| [`productAfter`](../master-data/product.md) | string | Business code of the product produced after the clean. | `"PRD044"` |
| `start` | string | Date and time when the clean started, in ISO 8601 format. | `"2026-08-11T06:00:00+02:00"` |
| `end` | string or null | Optional date and time when the clean ended. | `"2026-08-11T07:50:00+02:00"` |

</div>

> [!IMPORTANT]
> `code` must be unique. Two cleans cannot use the same code.
>
> `line`, `regime`, `productBefore`, and `productAfter` must reference existing records.

## Product transition

The product pair defines the changeover that the clean belongs to.

```text
PRD001
   │
   │ clean
   ▼
PRD044
```

This is important because cleaning requirements can depend on the direction of the product transition.

For example, cleaning from:

```text
PRD001 → PRD044
```

may require a different regime or expected duration than:

```text
PRD044 → PRD001
```

The actual clean can therefore be compared with the corresponding [Cleaning rule](../definitions-and-rules/cleaning-rule.md).

## Cleaning duration

The duration of the cleaning activity is determined from `start` and `end`.

For example:

```json
{
  "start": "2026-08-11T06:00:00+02:00",
  "end": "2026-08-11T07:50:00+02:00"
}
```

records a clean that took 110 minutes.

This actual duration can be compared with `expectedMinutes` from the applicable Cleaning rule.

## Planned and actual resource use

Cleaning can consume chemicals, water, energy, Labor, equipment time, and other resources.

Expected resource use can be recorded through [Planned use](planned-use.md).

Actual resource use is recorded separately through [Consumption](../operational-data/consumption.md).

This keeps the cleaning activity itself separate from the resources consumed while performing it.

## API resource

| Resource | Base path |
| --- | --- |
| `Clean` | `/services/pulse/food-beverage/cleans` |

## API methods

> [!NOTE]
> The API methods below follow the current Food & Beverage service pattern and are provisional until the Cleans implementation is available for verification.

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
| `code` | string | yes | Unique business code of the clean. |
| `line` | string | yes | Business code of the production line. |
| `regime` | string | yes | Business code of the cleaning regime. |
| `productBefore` | string | yes | Business code of the product produced before the clean. |
| `productAfter` | string | yes | Business code of the product produced after the clean. |
| `start` | string | yes | Date and time when the clean started. |
| `end` | string or null | no | Date and time when the clean ended. |


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

Partially updates an existing clean.

The fields to update are supplied in the `properties` object. The clean is identified by its business `code`.

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


### Retrieve a clean

`GET /services/pulse/food-beverage/cleans/select`

Returns the clean identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/cleans/select?id=CIP-260811-014
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the clean to retrieve. |

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

Returns cleans matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/cleans/query?line=LINE001&regime=allergen-cip
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `line` | string | no | Limits results to cleans on the specified production line. |
| `regime` | string | no | Limits results to the specified cleaning regime. |
| `from` | string | no | Limits results to cleans starting on or after the specified date or time. |
| `to` | string | no | Limits results to cleans starting on or before the specified date or time. |


### Delete a clean

`DELETE /services/pulse/food-beverage/cleans/delete`

Deletes the clean identified by its business code.

#### Request

```http
DELETE /services/pulse/food-beverage/cleans/delete?id=CIP-260811-014
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the clean to delete. |