# Consumption

Records resources actually used by a [Run](../production-activities/run.md), [Batch](../production-activities/batch.md), or [Clean](../production-activities/clean.md).

Consumption can represent ingredients, packaging, utilities, Labor, equipment, and other production or cleaning costs.

## The Consumption object

```json
{
  "usedBy": "BULK-260810-07",
  "category": "ingredient",
  "item": "MAT0001",
  "lot": "IN-260807-441",
  "quantity": 940,
  "unit": "kg",
  "unitValue": 3.12,
  "at": "2026-08-10T18:20:00+02:00"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `usedBy` | string | Business code of the Run, Batch, or Clean that used the resource. | `"BULK-260810-07"` |
| `category` | string | Resource category. See the supported categories below. | `"ingredient"` |
| `item` | string | Business code of the specific resource that was used. The expected resource type depends on `category`. | `"MAT0001"` |
| [`lot`](../production-activities/lot.md) | string or null | Optional Lot code identifying the specific Material quantity consumed. Required when the referenced Material is lot-tracked. | `"IN-260807-441"` |
| `quantity` | number | Quantity actually consumed. | `940` |
| `unit` | string | Unit in which the consumed quantity is expressed. | `"kg"` |
| `unitValue` | number or null | Optional cost per unit at the time the resource was used. | `3.12` |
| `at` | string or null | Optional approximate date and time of consumption. | `"2026-08-10T18:20:00+02:00"` |

</div>

> [!IMPORTANT]
> `usedBy`, `item`, `lot`, and `at` together identify a Consumption record.
>
> `usedBy` must reference an existing Run, Batch, or Clean.
>
> `item` must reference an existing resource of the type supported by the selected category.
>
> When a Material uses `lotTracked: true`, `lot` is required.
>
> When `lot` is supplied for a Material, the Lot must belong to that Material.

## Categories

Consumption uses the same resource categories as [Planned use](../production-activities/planned-use.md).

| Category | Item resource |
| --- | --- |
| `ingredient` | [Material](../master-data/material.md) |
| `packaging` | [Material](../master-data/material.md) |
| `chemical` | [Material](../master-data/material.md) |
| `water` | [Cost line](../master-data/cost-line.md) |
| `energy` | [Cost line](../master-data/cost-line.md) |
| `effluent` | [Cost line](../master-data/cost-line.md) |
| `labour` | [Crew](../master-data/crew.md) |
| `equipment` | [Machine](../master-data/machine.md) |
| `expense` | [Cost line](../master-data/cost-line.md) |

Each category is associated with a corresponding cost Measurement:

| Category | Cost measurement |
| --- | --- |
| `ingredient` | `ingredient-cost` |
| `packaging` | `packaging-cost` |
| `chemical` | `chemical-cost` |
| `water` | `water-cost` |
| `energy` | `energy-cost` |
| `effluent` | `effluent-cost` |
| `labour` | `labour-cost` |
| `equipment` | `equipment-cost` |
| `expense` | `expense-cost` |

The category determines both the cost Measurement and the resource type that `item` must reference.

### Examples

Labor consumption uses `category: "labour"` and a Crew as the `item`:

```json
{
  "usedBy": "L01-260810-002",
  "category": "labour",
  "item": "CREW-C",
  "quantity": 32,
  "unit": "h",
  "unitValue": 24.5,
  "at": "2026-08-11T05:50:00+02:00"
}
```

The public API category value remains `labour`.

A Clean can record its own resource use in the same way:

```json
{
  "usedBy": "CIP-260811-014",
  "category": "chemical",
  "item": "CAUSTIC-01",
  "quantity": 18,
  "unit": "kg",
  "at": "2026-08-11T06:20:00+02:00"
}
```

## Lot traceability

For lot-tracked Materials, include the specific Lot that was consumed:

```json
{
  "usedBy": "BULK-260810-07",
  "category": "ingredient",
  "item": "MAT0001",
  "lot": "IN-260807-441",
  "quantity": 940,
  "unit": "kg"
}
```

Pulse validates that the referenced Lot belongs to the Material identified by `item`.

For Materials that are not lot-tracked, `lot` can be omitted.

## Consumption time

`at` records the approximate time at which the resource was consumed.

When `at` is omitted, Pulse associates the Consumption internally with the start of the referenced activity.

The facade still returns:

```json
{
  "at": null
}
```

when no consumption timestamp was explicitly supplied.

## Planned and actual use

[Planned use](../production-activities/planned-use.md) describes what an activity was expected to consume.

Consumption records what was actually used.

For example:

```text
Planned use
MAT0042 — 300 kg

Actual consumption
MAT0042 — 318 kg
```

Keeping planned and actual records separate allows Pulse to compare expected and actual resource use for the same item.

## Reference protection

A resource or Lot referenced by an existing Consumption record cannot be deleted until the Consumption reference is removed.

## API resource

| Resource | Base path |
| --- | --- |
| `Consumption` | `/services/pulse/food-beverage/consumption` |

## API methods

### Create consumption

`POST /services/pulse/food-beverage/consumption/insert`

Records one actual Consumption row.

#### Request

```http
POST /services/pulse/food-beverage/consumption/insert
Content-Type: application/json
```

```json
{
  "usedBy": "BULK-260810-07",
  "category": "ingredient",
  "item": "MAT0001",
  "lot": "IN-260807-441",
  "quantity": 940,
  "unit": "kg",
  "unitValue": 3.12,
  "at": "2026-08-10T18:20:00+02:00"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `usedBy` | string | yes | Business code of the Run, Batch, or Clean. |
| `category` | string | yes | Resource category. |
| `item` | string | yes | Business code of the consumed resource. |
| `lot` | string or null | conditional | Required when the referenced Material is lot-tracked. |
| `quantity` | number | yes | Quantity actually consumed. |
| `unit` | string | yes | Unit of the consumed quantity. |
| `unitValue` | number or null | no | Cost per unit at the time of use. |
| `at` | string or null | no | Approximate date and time of consumption. |

### Update consumption

`PUT /services/pulse/food-beverage/consumption/update`

Updates an existing Consumption row.

The row is identified by `usedBy`, `item`, `lot`, and `at`.

`category` must match the existing Consumption row and cannot be changed through update.

#### Request

```http
PUT /services/pulse/food-beverage/consumption/update
Content-Type: application/json
```

```json
{
  "usedBy": "BULK-260810-07",
  "category": "ingredient",
  "item": "MAT0001",
  "lot": "IN-260807-441",
  "quantity": 960,
  "unit": "kg",
  "unitValue": 3.12,
  "at": "2026-08-10T18:20:00+02:00"
}
```

### Patch consumption

`PATCH /services/pulse/food-beverage/consumption/patch`

Partially updates an existing Consumption row.

`properties.usedBy` and `properties.item` are required.

Because `lot` and `at` are also part of the Consumption key, include them when identifying a row that has those values.

The key fields themselves are not changed by PATCH.

`unitValue` can be explicitly cleared by including it with a null value.

#### Request

```http
PATCH /services/pulse/food-beverage/consumption/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "usedBy": "BULK-260810-07",
    "item": "MAT0001",
    "lot": "IN-260807-441",
    "at": "2026-08-10T18:20:00+02:00",
    "quantity": 960
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.usedBy` | string | yes | Business code of the activity that identifies the Consumption row. |
| `properties.item` | string | yes | Business code of the item that identifies the Consumption row. |
| `properties.lot` | string or null | conditional | Lot component of the Consumption key. Include it when the row has a Lot. |
| `properties.at` | string or null | conditional | Time component of the Consumption key. Include it when the row has an explicit `at`. |
| `properties.category` | string | no | Existing category. The category cannot be changed. |
| `properties.quantity` | number | no | New consumed quantity. |
| `properties.unit` | string | no | New unit of the consumed quantity. |
| `properties.unitValue` | number or null | no | New cost per unit, or `null` to clear it. |

### Retrieve consumption

`GET /services/pulse/food-beverage/consumption/select`

Returns the Consumption row identified by its composite business key.

#### Request

```http
GET /services/pulse/food-beverage/consumption/select?usedBy=BULK-260810-07&item=MAT0001&lot=IN-260807-441&at=2026-08-10T18:20:00%2B02:00
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `usedBy` | string | yes | Business code of the Run, Batch, or Clean. |
| `item` | string | yes | Business code of the consumed item. |
| `lot` | string | no | Lot component of the Consumption key. |
| `at` | string | no | Time component of the Consumption key. |

### List consumption

`GET /services/pulse/food-beverage/consumption/query`

Returns Consumption rows matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/consumption/query?usedBy=BULK-260810-07&categories=ingredient
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `usedBy` | string or array of strings | no | Limits results to the specified Runs, Batches, or Cleans. |
| `categories` | string or array of strings | no | Limits results to the specified resource categories. |
| `items` | string or array of strings | no | Limits results to the specified item business codes. |
| `lots` | string or array of strings | no | Limits results to the specified Lot business codes. |
| `from` | string | no | Limits results to Consumption occurring at or after the specified date and time. |
| `to` | string | no | Limits results to Consumption occurring at or before the specified date and time. |

Multiple values can be supplied by repeating the query parameter:

```http
GET /services/pulse/food-beverage/consumption/query?categories=ingredient&categories=packaging
```

### Delete consumption

`DELETE /services/pulse/food-beverage/consumption/delete`

Deletes the Consumption row identified by its composite business key.

#### Request

```http
DELETE /services/pulse/food-beverage/consumption/delete?usedBy=BULK-260810-07&item=MAT0001&lot=IN-260807-441&at=2026-08-10T18:20:00%2B02:00
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `usedBy` | string | yes | Business code of the Run, Batch, or Clean. |
| `item` | string | yes | Business code of the consumed item. |
| `lot` | string | no | Lot component of the Consumption key. |
| `at` | string | no | Time component of the Consumption key. |