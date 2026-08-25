# Planned use

<!-- TODO: This page is currently based on ApiSurfaceRevised. Revisit it once the implementation is available and verify fields, routes, query parameters, PATCH behavior, and examples against the current code. -->

Defines the resources that a run, batch, or clean is expected to use.

Each planned-use record identifies one specific item, its planned quantity, and optionally its planned cost per unit.

## The Planned use object

```json
{
  "usedBy": "L01-260810-002",
  "category": "ingredient",
  "item": "MAT0042",
  "quantity": 300,
  "unit": "kg",
  "unitValue": 2.44
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `usedBy` | string | Business code of the [Run](run.md), [Batch](batch.md), or [Clean](clean.md) that is expected to use the item. | `"L01-260810-002"` |
| `category` | string | Resource category. See the supported categories below. | `"ingredient"` |
| `item` | string | Business code of the specific Material, Cost line, Crew, or Machine being planned. | `"MAT0042"` |
| `quantity` | number | Planned quantity of the item. | `300` |
| `unit` | string | Unit in which the planned quantity is expressed. | `"kg"` |
| `unitValue` | number or null | Optional planned cost per unit. | `2.44` |

</div>

> [!IMPORTANT]
> `usedBy`, `category`, and `item` together identify a planned-use record.
>
> `item` is required. Category-level totals without a specific item are not supported.

## Categories

Supported categories are:

| Category | Purpose |
| --- | --- |
| `ingredient` | Ingredients used during production. |
| `packaging` | Packaging materials. |
| `chemical` | Cleaning or process chemicals. |
| `water` | Water consumption. |
| `energy` | Energy consumption. |
| `effluent` | Effluent or waste-water related use. |
| `labour` | Labor resources. |
| `equipment` | Equipment resources. |
| `expense` | Other defined production expenses. |

Each category is associated with a corresponding cost measure in Pulse:

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

## Item-level planning

Planned use is recorded for a specific item rather than only for a category.

For example:

```json
{
  "usedBy": "L01-260810-002",
  "category": "ingredient",
  "item": "MAT0042",
  "quantity": 300,
  "unit": "kg"
}
```

is valid because the planned quantity belongs to a specific material.

A category-only plan such as:

```json
{
  "usedBy": "L01-260810-002",
  "category": "ingredient",
  "quantity": 12200,
  "unit": "kg"
}
```

is not supported.

Items within the same category may use different units. Planning each item separately allows planned quantities to be compared directly with the corresponding actual use.

Category totals can then be derived from the individual records where appropriate.

## Planned and actual use

Planned use describes what a production activity was expected to consume.

Actual use is submitted separately through [Consumption](../operational-data/consumption.md).

For example:

```text
Planned use
    ↓
MAT0042 — 300 kg

Actual consumption
    ↓
MAT0042 — 318 kg
```

Keeping the planned and actual records separate allows Pulse to compare expected and actual resource use for the same item.

## API resource

| Resource | Base path |
| --- | --- |
| `Planned use` | `/services/pulse/food-beverage/planned-use` |

## API methods

> [!NOTE]
> The API methods below follow the current Food & Beverage service pattern and are provisional until the Planned use implementation is available for verification.

### Create planned use

`POST /services/pulse/food-beverage/planned-use/insert`

Creates a planned-use record for a run, batch, or clean.

#### Request

```http
POST /services/pulse/food-beverage/planned-use/insert
Content-Type: application/json
```

```json
{
  "usedBy": "L01-260810-002",
  "category": "ingredient",
  "item": "MAT0042",
  "quantity": 300,
  "unit": "kg",
  "unitValue": 2.44
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `usedBy` | string | yes | Business code of the run, batch, or clean. |
| `category` | string | yes | Resource category. |
| `item` | string | yes | Business code of the specific resource being planned. |
| `quantity` | number | yes | Planned quantity. |
| `unit` | string | yes | Unit of the planned quantity. |
| `unitValue` | number or null | no | Planned cost per unit. |


### Update planned use

`PUT /services/pulse/food-beverage/planned-use/update`

Updates an existing planned-use record.

The record is identified by the combination of `usedBy`, `category`, and `item`.

#### Request

```http
PUT /services/pulse/food-beverage/planned-use/update
Content-Type: application/json
```

```json
{
  "usedBy": "L01-260810-002",
  "category": "ingredient",
  "item": "MAT0042",
  "quantity": 320,
  "unit": "kg",
  "unitValue": 2.44
}
```


### Patch planned use

`PATCH /services/pulse/food-beverage/planned-use/patch`

Partially updates an existing planned-use record.

The exact PATCH identification shape still needs to be verified against the implementation.

#### Request

```http
PATCH /services/pulse/food-beverage/planned-use/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "usedBy": "L01-260810-002",
    "category": "ingredient",
    "item": "MAT0042",
    "quantity": 320
  }
}
```


### Retrieve planned use

`GET /services/pulse/food-beverage/planned-use/select`

Returns a planned-use record identified by its subject, category, and item.

#### Request

```http
GET /services/pulse/food-beverage/planned-use/select?usedBy=L01-260810-002&category=ingredient&item=MAT0042
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `usedBy` | string | yes | Business code of the run, batch, or clean. |
| `category` | string | yes | Resource category. |
| `item` | string | yes | Business code of the planned item. |


### List planned use

`GET /services/pulse/food-beverage/planned-use/query`

Returns planned-use records matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/planned-use/query?usedBy=L01-260810-002&category=ingredient
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `usedBy` | string | no | Limits results to a specific run, batch, or clean. |
| `category` | string | no | Limits results to a specific resource category. |
| `item` | string | no | Limits results to a specific planned item. |


### Delete planned use

`DELETE /services/pulse/food-beverage/planned-use/delete`

Deletes a planned-use record.

#### Request

```http
DELETE /services/pulse/food-beverage/planned-use/delete?usedBy=L01-260810-002&category=ingredient&item=MAT0042
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `usedBy` | string | yes | Business code of the run, batch, or clean. |
| `category` | string | yes | Resource category. |
| `item` | string | yes | Business code of the planned item. |