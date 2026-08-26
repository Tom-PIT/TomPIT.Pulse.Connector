# Product limit

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
  "to": null,
  "setBy": "Product specification rev 4"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| [`product`](../master-data/product.md) | string | Business code of the product to which the limit applies. | `"PRD001"` |
| [`measure`](measurement.md) | string | Business code of the measurement constrained by the limit. | `"fat"` |
| `min` | number or null | Optional minimum acceptable value. | `3.4` |
| `target` | number or null | Optional target value. | `3.6` |
| `max` | number or null | Optional maximum acceptable value. | `3.8` |
| `unit` | string | Unit associated with the referenced measurement. | `"%"` |
| `stopsRelease` | boolean | Indicates whether the limit is marked as release-stopping. | `false` |
| `from` | string | ISO 8601 timestamp from which this revision is in force. | `"2026-01-01T00:00:00+01:00"` |
| `to` | string or null | ISO 8601 timestamp at which this revision was superseded. `null` while the revision remains effective. | `null` |
| `setBy` | string or null | Optional source or authority that established the limit. | `"Product specification rev 4"` |

</div>

> [!IMPORTANT]
> `product` must reference an existing product and `measure` must reference an existing measurement.
>
> `unit` should match the unit declared for the referenced measurement.

## Effective dates and revisions

A Product limit revision is identified by the combination of:

- `product`
- `measure`
- `from`

Submitting the same combination again corrects that revision.

Submitting the same product and measurement with a later `from` timestamp creates a new revision. Pulse closes the previous revision automatically when necessary.

The earlier revision remains applicable before the new revision takes effect.

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

## Examples

Product limits can represent different kinds of product-specific specifications.

### Declared weight

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

### Shelf life

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

### Create a product limit

`POST /services/pulse/food-beverage/product-limits/insert`

Creates a new Product limit revision or corrects an existing revision with the same `product`, `measure`, and `from` values.

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
| `unit` | string | yes | Unit associated with the referenced measurement. |
| `stopsRelease` | boolean | no | Indicates whether the limit is marked as release-stopping. |
| `from` | string | yes | ISO 8601 timestamp from which this revision is in force. |
| `setBy` | string or null | no | Source or authority that established the limit. |

### Correct or revise a product limit

Product limits do not use a separate update endpoint. Correct or revise a limit by submitting it again through the `insert` endpoint.

To correct an existing revision, submit the same `product`, `measure`, and `from` values with the corrected limit values.

To create a new revision, submit the same `product` and `measure` with a new `from` timestamp. Pulse closes the previous revision automatically when necessary.

#### Correct an existing revision

```json
{
  "product": "PRD001",
  "measure": "fat",
  "min": 3.5,
  "target": 3.6,
  "max": 3.8,
  "unit": "%",
  "from": "2026-01-01T00:00:00+01:00",
  "setBy": "Product specification rev 4"
}
```

#### Create a new revision

```json
{
  "product": "PRD001",
  "measure": "fat",
  "min": 3.5,
  "target": 3.7,
  "max": 3.9,
  "unit": "%",
  "from": "2026-07-01T00:00:00+02:00",
  "setBy": "Product specification rev 5"
}
```


### List product limits

`GET /services/pulse/food-beverage/product-limits/query`

Returns product limits matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/product-limits/query?products=PRD001&measures=fat
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `products` | string or array of strings | no | Limits results to the specified product business codes. |
| `measures` | string or array of strings | no | Limits results to the specified measurement business codes. |
| `inForceAt` | string | no | Limits results to revisions that were in force at the specified ISO 8601 timestamp. |