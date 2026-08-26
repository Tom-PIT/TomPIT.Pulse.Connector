# Planned use

Defines the resources that a [Run](run.md), [Batch](batch.md), or [Clean](clean.md) is expected to use.

Each Planned-use record identifies one specific item, its planned quantity, and optionally its planned cost per unit.

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
| `usedBy` | string | Business code of the Run, Batch, or Clean that is expected to use the item. | `"L01-260810-002"` |
| `category` | string | Resource category. See the supported categories below. | `"ingredient"` |
| `item` | string | Business code of the specific resource being planned. The expected resource type depends on `category`. | `"MAT0042"` |
| `quantity` | number | Planned quantity of the item. | `300` |
| `unit` | string | Unit in which the planned quantity is expressed. | `"kg"` |
| `unitValue` | number or null | Optional planned cost per unit. | `2.44` |

</div>

> [!IMPORTANT]
> `usedBy`, `category`, and `item` together identify a Planned-use record.
>
> `usedBy` must reference an existing Run, Batch, or Clean.
>
> `item` must reference an existing resource of the type supported by the selected category.

## Categories

Supported categories are:

| Category | Item resource | Purpose |
| --- | --- | --- |
| `ingredient` | [Material](../master-data/material.md) | Ingredients used during production. |
| `packaging` | [Material](../master-data/material.md) | Packaging materials. |
| `chemical` | [Material](../master-data/material.md) | Cleaning or process chemicals. |
| `water` | [Cost line](../master-data/cost-line.md) | Water use. |
| `energy` | [Cost line](../master-data/cost-line.md) | Energy use. |
| `effluent` | [Cost line](../master-data/cost-line.md) | Effluent or waste-water related use. |
| `labour` | [Crew](../master-data/crew.md) | Labor resources. |
| `equipment` | [Machine](../master-data/machine.md) | Equipment resources. |
| `expense` | [Cost line](../master-data/cost-line.md) | Other defined production expenses. |

Each category is associated with a corresponding cost Measurement in Pulse:

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

is valid because the planned quantity belongs to a specific Material.

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

The category also determines what kind of resource `item` must reference. For example, an `ingredient` item must be a Material, while an `equipment` item must be a Machine.

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

## Reference protection

A Material, Cost line, Crew, or Machine referenced by an existing Planned-use record cannot be deleted until the reference is removed.

## API resource

| Resource | Base path |
| --- | --- |
| `Planned use` | `/services/pulse/food-beverage/planned-use` |

## API methods

### Create planned use

`POST /services/pulse/food-beverage/planned-use/insert`

Creates a Planned-use record for a Run, Batch, or Clean.

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
| `usedBy` | string | yes | Business code of the Run, Batch, or Clean. |
| `category` | string | yes | Resource category. |
| `item` | string | yes | Business code of the resource supported by the selected category. |
| `quantity` | number | yes | Planned quantity. |
| `unit` | string | yes | Unit of the planned quantity. |
| `unitValue` | number or null | no | Planned cost per unit. |

### Update planned use

`PUT /services/pulse/food-beverage/planned-use/update`

Updates an existing Planned-use record.

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

Partially updates an existing Planned-use record.

The record is identified by `properties.usedBy`, `properties.category`, and `properties.item`. All three are required.

`quantity` and `unit` are preserved when omitted. `unitValue` can be explicitly cleared by including it with a null value.

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

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.usedBy` | string | yes | Business code of the Run, Batch, or Clean. |
| `properties.category` | string | yes | Resource category. |
| `properties.item` | string | yes | Business code of the planned item. |
| `properties.quantity` | number | no | New planned quantity. |
| `properties.unit` | string | no | New unit of the planned quantity. |
| `properties.unitValue` | number or null | no | New planned cost per unit, or `null` to clear it. |

### Retrieve planned use

`GET /services/pulse/food-beverage/planned-use/select`

Returns the Planned-use record identified by its subject, category, and item.

#### Request

```http
GET /services/pulse/food-beverage/planned-use/select?usedBy=L01-260810-002&category=ingredient&item=MAT0042
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `usedBy` | string | yes | Business code of the Run, Batch, or Clean. |
| `category` | string | yes | Resource category. |
| `item` | string | yes | Business code of the planned item. |

### List planned use

`GET /services/pulse/food-beverage/planned-use/query`

Returns Planned-use records matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/planned-use/query?usedBy=L01-260810-002&categories=ingredient
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `usedBy` | string or array of strings | no | Limits results to the specified Runs, Batches, or Cleans. |
| `categories` | string or array of strings | no | Limits results to the specified resource categories. |
| `items` | string or array of strings | no | Limits results to the specified planned-item business codes. |

Multiple values can be supplied by repeating the query parameter:

```http
GET /services/pulse/food-beverage/planned-use/query?categories=ingredient&categories=packaging
```

### Delete planned use

`DELETE /services/pulse/food-beverage/planned-use/delete`

Deletes the Planned-use record identified by its subject, category, and item.

#### Request

```http
DELETE /services/pulse/food-beverage/planned-use/delete?usedBy=L01-260810-002&category=ingredient&item=MAT0042
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `usedBy` | string | yes | Business code of the Run, Batch, or Clean. |
| `category` | string | yes | Resource category. |
| `item` | string | yes | Business code of the planned item. |