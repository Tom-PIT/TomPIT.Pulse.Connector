<!-- TODO: This page is currently based on ApiSurfaceRevised. Revisit it once the implementation is available and verify fields, routes, query parameters, PATCH behavior, and examples against the current code. -->

# Product limits

Defines product specifications and limits that apply from a specific point in time.

Product limits can define permitted ranges, target values, legal pack weights, shelf life, and critical limits that can prevent product release.

## The Product limit object

```json
{
  "product": "PRD001",
  "measure": "fat",
  "min": 3.4,
  "target": 3.6,
  "max": 3.8,
  "unit": "%",
  "stopsRelease": false,
  "from": "2026-01-01T00:00:00+01:00",
  "setBy": "Product specification rev 4"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| [`product`](../master-data/product.md) | string | Business code of the product to which the limit applies. | `"PRD001"` |
| [`measure`](measurements.md) | string | Measurement code that the limit applies to. | `"fat"` |
| `min` | number or null | Optional minimum permitted or expected value. | `3.4` |
| `target` | number or null | Optional target value. | `3.6` |
| `max` | number or null | Optional maximum permitted or expected value. | `3.8` |
| `unit` | string | Unit of the submitted values. Used to validate the values against the measurement definition. | `"%"` |
| `stopsRelease` | boolean | Indicates whether a breach of the limit should prevent product release. | `false` |
| `from` | string | ISO 8601 timestamp from which this revision is in force. | `"2026-01-01T00:00:00+01:00"` |
| `setBy` | string or null | Optional source or authority that established the limit. | `"Product specification rev 4"` |

</div>

> [!IMPORTANT]
> At least one of `min`, `target`, or `max` must be provided.
>
> `product` must reference an existing product and `measure` must reference an existing measurement.
>
> `unit` must be compatible with the unit declared for the measurement.

See [Types and attributes](../master-data/types-and-attributes.md) for guidance on extensible master-data properties.

## Effective dates and revisions

A product limit is identified by the combination of:

- `product`
- `measure`
- `from`

A later `from` value represents a new revision of the limit.

For example, if a product specification changes on 1 July, register a new limit beginning on that date rather than changing the limit that applied before July.

This preserves the specification that was in force when earlier production occurred.

```json
{
  "product": "PRD001",
  "measure": "fat",
  "min": 3.4,
  "target": 3.6,
  "max": 3.8,
  "unit": "%",
  "from": "2026-01-01T00:00:00+01:00"
}
```

A later revision might be:

```json
{
  "product": "PRD001",
  "measure": "fat",
  "min": 3.5,
  "target": 3.7,
  "max": 3.9,
  "unit": "%",
  "from": "2026-07-01T00:00:00+02:00"
}
```

The earlier revision remains applicable to production before the new revision took effect.

## Critical product limits

Set `stopsRelease` to `true` for a product limit that represents a critical release condition.

For example:

```json
{
  "product": "PRD001",
  "measure": "pasteurisation-temp",
  "min": 72,
  "unit": "C",
  "stopsRelease": true,
  "from": "2026-01-01T00:00:00+01:00",
  "setBy": "HACCP plan rev 7"
}
```

A breach of such a limit can be used to trigger the product-release behaviour associated with critical control points.

## Declared weight

The legal or declared pack weight is represented as a product limit using the appropriate measurement.

For example:

```json
{
  "product": "PRD001",
  "measure": "declared-weight",
  "target": 125,
  "unit": "g",
  "from": "2026-01-01T00:00:00+01:00",
  "setBy": "Legal metrology"
}
```

Keeping declared weight as a time-effective limit means that a later pack-size change does not change the specification that applied to earlier production.

## Shelf life

Shelf life is also represented as a product limit.

```json
{
  "product": "PRD001",
  "measure": "shelf-life-days",
  "target": 21,
  "unit": "days",
  "from": "2026-01-01T00:00:00+01:00"
}
```

Pulse can use the shelf-life value in force at production time when determining the expected expiry of produced lots.

## API resource

| Resource | Base path |
| --- | --- |
| `Product limit` | `/services/pulse/food-beverage/product-limits` |

## API methods

> [!NOTE]
> The API methods below follow the current Food & Beverage service pattern and are provisional until the Product limits implementation is available for verification.

### Create a product limit

`POST /services/pulse/food-beverage/product-limits/insert`

Creates a new product-limit revision.

#### Request

```http
POST /services/pulse/food-beverage/product-limits/insert
Content-Type: application/json
```

```json
{
  "product": "PRD001",
  "measure": "fat",
  "min": 3.4,
  "target": 3.6,
  "max": 3.8,
  "unit": "%",
  "stopsRelease": false,
  "from": "2026-01-01T00:00:00+01:00",
  "setBy": "Product specification rev 4"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `product` | string | yes | Business code of the product. |
| `measure` | string | yes | Measurement code that the limit applies to. |
| `min` | number or null | no | Minimum value. |
| `target` | number or null | no | Target value. |
| `max` | number or null | no | Maximum value. |
| `unit` | string | yes | Unit used to validate the submitted values. |
| `stopsRelease` | boolean | no | Indicates whether a breach prevents product release. |
| `from` | string | yes | ISO 8601 timestamp from which this revision is in force. |
| `setBy` | string or null | no | Source or authority that established the limit. |


### Update a product limit

`PUT /services/pulse/food-beverage/product-limits/update`

Updates an existing product-limit revision.

The revision is identified by its product, measurement, and effective-from timestamp.

#### Request

```http
PUT /services/pulse/food-beverage/product-limits/update
Content-Type: application/json
```

```json
{
  "product": "PRD001",
  "measure": "fat",
  "min": 3.5,
  "target": 3.6,
  "max": 3.8,
  "unit": "%",
  "stopsRelease": false,
  "from": "2026-01-01T00:00:00+01:00",
  "setBy": "Product specification rev 4"
}
```


### Patch a product limit

`PATCH /services/pulse/food-beverage/product-limits/patch`

Partially updates an existing product-limit revision.

The exact PATCH identification shape still needs to be verified against the implementation.

#### Request

```http
PATCH /services/pulse/food-beverage/product-limits/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "product": "PRD001",
    "measure": "fat",
    "from": "2026-01-01T00:00:00+01:00",
    "max": 3.9
  }
}
```


### Retrieve a product limit

`GET /services/pulse/food-beverage/product-limits/select`

Returns a product-limit revision.

The revised facade specification states that Product limits use a stable code derived from `(product, measure, from)`.

#### Request

```http
GET /services/pulse/food-beverage/product-limits/select?id=PRD001/fat/2026-01-01
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Stable product-limit code derived from the product, measurement, and effective date. |


### List product limits

`GET /services/pulse/food-beverage/product-limits/query`

Returns product limits matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/product-limits/query?product=PRD001&measure=fat
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `product` | string | no | Limits results to a product. |
| `measure` | string | no | Limits results to a measurement. |
| `inForceAt` | string | no | Returns the revision in force at the specified time. |


### Delete a product limit

`DELETE /services/pulse/food-beverage/product-limits/delete`

Deletes the specified product-limit revision.

#### Request

```http
DELETE /services/pulse/food-beverage/product-limits/delete?id=PRD001/fat/2026-01-01
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Stable product-limit code derived from the product, measurement, and effective date. |