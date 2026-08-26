# Parts and Labor

Records planned and actual resources used for a maintenance [Work order](work-order.md).

The resource can represent Labor, spare parts, equipment, utilities, chemicals, and other maintenance costs.

## The Parts and Labor object

```json
{
  "workOrder": "WO-8830",
  "category": "labour",
  "item": "CREW-MAINT",
  "plannedQuantity": 3,
  "actualQuantity": 3.4,
  "unit": "h",
  "unitValue": 31,
  "at": "2026-08-14T11:20:00+02:00"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| [`workOrder`](work-order.md) | string | Business code of the Work order that planned or used the resource. | `"WO-8830"` |
| `category` | string | Resource category. See the supported categories below. | `"labour"` |
| `item` | string | Business code of the specific resource. The expected resource type depends on `category`. | `"CREW-MAINT"` |
| [`lot`](../production-activities/lot.md) | string or null | Optional Lot code identifying the specific Material quantity used. Required for lot-tracked Materials. | `"IN-260701-088"` |
| `plannedQuantity` | number or null | Quantity planned for the Work order. | `3` |
| `actualQuantity` | number or null | Quantity actually used. | `3.4` |
| `unit` | string | Unit in which planned and actual quantities are expressed. | `"h"` |
| `unitValue` | number or null | Optional cost per unit. | `31` |
| `at` | string or null | Optional date and time associated with actual use. | `"2026-08-14T11:20:00+02:00"` |

</div>

> [!IMPORTANT]
> `workOrder`, `category`, `item`, and `lot` together identify a Parts and Labor record.
>
> `workOrder` must reference an existing Work order.
>
> `item` must reference an existing resource of the type supported by the selected category.
>
> At least one of `plannedQuantity` or `actualQuantity` must be provided.
>
> When a Material is lot-tracked, `lot` is required and the Lot must belong to that Material.

## Categories

Supported categories are:

| Category | Item resource | Purpose |
| --- | --- | --- |
| `ingredient` | [Material](../master-data/material.md) | Spare parts, consumables, or other maintenance Materials. |
| `labour` | [Crew](../master-data/crew.md) | Labor resources. |
| `equipment` | [Machine](../master-data/machine.md) | Equipment resources. |
| `expense` | [Cost line](../master-data/cost-line.md) | Other maintenance expenses. |
| `chemical` | [Material](../master-data/material.md) | Chemicals used during maintenance. |
| `water` | [Cost line](../master-data/cost-line.md) | Water use. |
| `energy` | [Cost line](../master-data/cost-line.md) | Energy use. |

The API category value remains `labour`, while public prose uses **Labor**.

Each category is associated with a corresponding cost Measurement:

| Category | Cost measurement |
| --- | --- |
| `ingredient` | `ingredient-cost` |
| `labour` | `labour-cost` |
| `equipment` | `equipment-cost` |
| `expense` | `expense-cost` |
| `chemical` | `chemical-cost` |
| `water` | `water-cost` |
| `energy` | `energy-cost` |

## Planned and actual quantities

A single Parts and Labor record can contain both the planned and actual quantity:

```json
{
  "workOrder": "WO-8830",
  "category": "labour",
  "item": "CREW-MAINT",
  "plannedQuantity": 3,
  "actualQuantity": 3.4,
  "unit": "h",
  "unitValue": 31
}
```

This preserves the two values separately:

```text
Planned: 3.0 h
Actual:  3.4 h
Variance: +0.4 h
```

`plannedQuantity` describes what was expected.

`actualQuantity` describes what was actually used.

Either value can exist without the other.

## Unplanned work

For unplanned maintenance, there may be no planned quantity.

For example:

```json
{
  "workOrder": "WO-8891",
  "category": "ingredient",
  "item": "MAT-GASKET-P3",
  "actualQuantity": 2,
  "unit": "pcs",
  "unitValue": 4.2
}
```

Omitting `plannedQuantity` can simply mean that no resource quantity was planned before the work was performed.

## Planned-only resources

A resource can also be planned before any actual use has been reported:

```json
{
  "workOrder": "WO-8830",
  "category": "ingredient",
  "item": "MAT-GASKET-P3",
  "plannedQuantity": 8,
  "unit": "pcs",
  "unitValue": 4.2
}
```

`actualQuantity` can be added later when the work is completed or the resource is consumed.

## Parts and lot traceability

When a maintenance Material comes from a traceable Lot, include the Lot code:

```json
{
  "workOrder": "WO-8830",
  "category": "ingredient",
  "item": "MAT-GASKET-P3",
  "lot": "IN-260701-088",
  "plannedQuantity": 8,
  "actualQuantity": 8,
  "unit": "pcs",
  "unitValue": 4.2,
  "at": "2026-08-14T11:20:00+02:00"
}
```

For a lot-tracked Material, `lot` is required.

Pulse also verifies that the supplied Lot belongs to the Material identified by `item`.

## Actual-use time

`at` records when the actual resource use occurred.

When `actualQuantity` is supplied but `at` is omitted, Pulse associates the actual use internally with the start of the Work order.

The facade still returns:

```json
{
  "at": null
}
```

when no actual-use timestamp was explicitly submitted.

## Labor

Labor is recorded at Crew level.

```json
{
  "workOrder": "WO-8830",
  "category": "labour",
  "item": "CREW-MAINT",
  "plannedQuantity": 3,
  "actualQuantity": 3.4,
  "unit": "h",
  "unitValue": 31
}
```

The `item` identifies a registered Crew rather than an individual person.

## Relationship to production resource use

Parts and Labor follows the same general planned-versus-actual pattern used elsewhere in the Food & Beverage model:

```text
Production activities
  Planned use
      ↓
  Consumption

Maintenance
  Parts and Labor
      ↓
  plannedQuantity / actualQuantity
```

For maintenance, both quantities are exposed through the same resource because they belong to the same Work order, category, item, and optional Lot.

## API resource

| Resource | Base path |
| --- | --- |
| `Parts and Labor` | `/services/pulse/food-beverage/parts-and-labour` |

## API methods

### Submit parts and Labor

`POST /services/pulse/food-beverage/parts-and-labour/insert`

Records one planned and/or actual maintenance resource row.

If a row with the same `workOrder`, `category`, `item`, and `lot` already exists, Pulse synchronizes the supplied values with that row.

#### Request

```http
POST /services/pulse/food-beverage/parts-and-labour/insert
Content-Type: application/json
```

```json
{
  "workOrder": "WO-8830",
  "category": "labour",
  "item": "CREW-MAINT",
  "plannedQuantity": 3,
  "actualQuantity": 3.4,
  "unit": "h",
  "unitValue": 31,
  "at": "2026-08-14T11:20:00+02:00"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `workOrder` | string | yes | Business code of the Work order. |
| `category` | string | yes | Resource category. |
| `item` | string | yes | Business code of the resource supported by the selected category. |
| `lot` | string or null | conditional | Required when `item` is a lot-tracked Material. |
| `plannedQuantity` | number or null | conditional | Planned resource quantity. At least one of `plannedQuantity` or `actualQuantity` is required. |
| `actualQuantity` | number or null | conditional | Actual resource quantity. At least one of `plannedQuantity` or `actualQuantity` is required. |
| `unit` | string | yes | Unit of the planned and actual quantities. |
| `unitValue` | number or null | no | Cost per unit. |
| `at` | string or null | no | Date and time associated with actual use. |

### Update parts and Labor

`PUT /services/pulse/food-beverage/parts-and-labour/update`

Updates the row identified by `workOrder`, `category`, `item`, and `lot`.

#### Request

```http
PUT /services/pulse/food-beverage/parts-and-labour/update
Content-Type: application/json
```

```json
{
  "workOrder": "WO-8830",
  "category": "labour",
  "item": "CREW-MAINT",
  "lot": null,
  "plannedQuantity": 3,
  "actualQuantity": 3.6,
  "unit": "h",
  "unitValue": 31,
  "at": "2026-08-14T11:25:00+02:00"
}
```

### Patch parts and Labor

`PATCH /services/pulse/food-beverage/parts-and-labour/patch`

Partially updates an existing Parts and Labor row.

The row is identified by `properties.workOrder`, `properties.category`, `properties.item`, and `properties.lot`.

All four key properties must be present. When the row has no Lot, supply:

```json
{
  "lot": null
}
```

Fields omitted from `properties` keep their current values.

`plannedQuantity`, `actualQuantity`, `unitValue`, and `at` can be explicitly cleared by including them with a null value.

#### Request

```http
PATCH /services/pulse/food-beverage/parts-and-labour/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "workOrder": "WO-8830",
    "category": "labour",
    "item": "CREW-MAINT",
    "lot": null,
    "actualQuantity": 3.6
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.workOrder` | string | yes | Work order business code identifying the row. |
| `properties.category` | string | yes | Resource category identifying the row. |
| `properties.item` | string | yes | Item business code identifying the row. |
| `properties.lot` | string or null | yes | Lot component of the composite key. Use `null` when the row has no Lot. |
| `properties.plannedQuantity` | number or null | no | New planned quantity, or `null` to remove it. |
| `properties.actualQuantity` | number or null | no | New actual quantity, or `null` to remove it. |
| `properties.unit` | string | no | New quantity unit. |
| `properties.unitValue` | number or null | no | New cost per unit, or `null` to clear it. |
| `properties.at` | string or null | no | New actual-use timestamp, or `null` to clear the explicitly supplied timestamp. |

> [!IMPORTANT]
> After an update or patch, at least one of `plannedQuantity` or `actualQuantity` must remain if the row is expected to continue to exist.
>
> Clearing both removes both the planned and actual representation of the row.

### Retrieve parts and Labor

`GET /services/pulse/food-beverage/parts-and-labour/select`

Returns the Parts and Labor row identified by its composite business key.

#### Request

For a row without a Lot:

```http
GET /services/pulse/food-beverage/parts-and-labour/select?workOrder=WO-8830&category=labour&item=CREW-MAINT
```

For a row with a Lot:

```http
GET /services/pulse/food-beverage/parts-and-labour/select?workOrder=WO-8830&category=ingredient&item=MAT-GASKET-P3&lot=IN-260701-088
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `workOrder` | string | yes | Business code of the Work order. |
| `category` | string | yes | Resource category. |
| `item` | string | yes | Business code of the resource. |
| `lot` | string | no | Lot component of the composite key. |

### List parts and Labor

`GET /services/pulse/food-beverage/parts-and-labour/query`

Returns Parts and Labor rows matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/parts-and-labour/query?workOrders=WO-8830&categories=labour
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `workOrders` | string or array of strings | no | Limits results to the specified Work orders. |
| `categories` | string or array of strings | no | Limits results to the specified resource categories. |
| `items` | string or array of strings | no | Limits results to the specified resource business codes. |
| `lots` | string or array of strings | no | Limits results to the specified Lot business codes. |
| `from` | string | no | Earliest actual-use timestamp to include. |
| `to` | string | no | Latest actual-use timestamp to include. |

When `from` or `to` is supplied, only rows with matching actual use are returned. Planned-only rows are not included in a time-bounded query.

Multiple values can be supplied by repeating the query parameter:

```http
GET /services/pulse/food-beverage/parts-and-labour/query?categories=labour&categories=equipment
```

### Delete parts and Labor

`DELETE /services/pulse/food-beverage/parts-and-labour/delete`

Deletes both the planned and actual values represented by the Parts and Labor row.

#### Request

For a row without a Lot:

```http
DELETE /services/pulse/food-beverage/parts-and-labour/delete?workOrder=WO-8830&category=labour&item=CREW-MAINT
```

For a row with a Lot:

```http
DELETE /services/pulse/food-beverage/parts-and-labour/delete?workOrder=WO-8830&category=ingredient&item=MAT-GASKET-P3&lot=IN-260701-088
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `workOrder` | string | yes | Business code of the Work order. |
| `category` | string | yes | Resource category. |
| `item` | string | yes | Business code of the resource. |
| `lot` | string | no | Lot component of the composite key. |