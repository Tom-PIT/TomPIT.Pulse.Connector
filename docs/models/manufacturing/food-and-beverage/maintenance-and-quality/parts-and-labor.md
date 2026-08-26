# Parts and Labor

<!-- TODO: This page is currently based on ApiSurfaceRevised. Revisit it once the implementation is available and verify fields, routes, batching behavior, correction rules, and examples against the current code. -->

Records planned and actual resources used for a maintenance Work order.

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
| [`workOrder`](work-order.md) | string | Business code of the Work order that used or planned the resource. | `"WO-8830"` |
| `category` | string | Resource category. See the supported categories below. | `"labour"` |
| `item` | string | Business code of the specific Material, Crew, Machine, or Cost line. | `"CREW-MAINT"` |
| [`lot`](../production-activities/lot.md) | string or null | Optional lot code identifying the specific material delivery used. | `"IN-260701-088"` |
| `plannedQuantity` | number or null | Optional quantity that was planned for the Work order. | `3` |
| `actualQuantity` | number or null | Optional quantity actually used. | `3.4` |
| `unit` | string | Unit in which planned and actual quantities are expressed. | `"h"` |
| `unitValue` | number or null | Optional cost per unit. | `31` |
| `at` | string or null | Optional date and time associated with the actual use, typically when the work finished. | `"2026-08-14T11:20:00+02:00"` |

</div>

> [!IMPORTANT]
> `workOrder`, `category`, `item`, and `lot` together identify a Parts and Labor record.
>
> `workOrder` must reference an existing Work order.
>
> At least one of `plannedQuantity` or `actualQuantity` should be provided.

## Categories

Supported categories are:

| Category | `item` identifies |
| --- | --- |
| `ingredient` | A [Material](../master-data/material.md), such as a spare part or consumable. |
| `labour` | A [Crew](../master-data/crew.md). |
| `equipment` | A [Machine](../master-data/machine.md). |
| `expense` | A [Cost line](../master-data/cost-line.md). |
| `chemical` | A [Material](../master-data/material.md) used as a chemical. |
| `water` | A water resource. |
| `energy` | An energy resource. |

The API category value remains `labour`, while public prose uses **Labor**.

## Planned and actual quantities

A single request can carry both what was planned and what was actually used:

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

This makes the variance visible at integration time:

```text
Planned: 3.0 h
Actual:  3.4 h
Variance: +0.4 h
```

The planned and actual quantities remain separate concepts.

A planned quantity represents what was expected.

An actual quantity represents what was consumed.

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

Omitting `plannedQuantity` does not mean that the data is incomplete. It can simply mean that the work had no resource plan.

## Parts and lot traceability

When a maintenance part or consumable comes from a traceable lot, include the lot code:

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

This preserves the relationship between the Work order and the specific material delivery used during maintenance.

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

Parts and Labor follows the same basic planned-versus-actual pattern used elsewhere in the Food & Beverage model:

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

The difference is that both maintenance quantities are submitted through the same resource because they belong to the same Work order and item.

## Batching

The specification shows Parts and Labor accepting multiple records in one request:

```json
[
  {
    "workOrder": "WO-8830",
    "category": "labour",
    "item": "CREW-MAINT",
    "plannedQuantity": 3,
    "actualQuantity": 3.4,
    "unit": "h",
    "unitValue": 31,
    "at": "2026-08-14T11:20:00+02:00"
  },
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
]
```

The exact batching behavior should be verified once the implementation is available.

## API resource

| Resource | Base path |
| --- | --- |
| `Parts and Labor` | `/services/pulse/food-beverage/parts-and-labour` |

## API methods

> [!NOTE]
> The API methods below are provisional until the Parts and Labor implementation is available for verification.

### Submit parts and Labor

`POST /services/pulse/food-beverage/parts-and-labour/insert`

Records planned and/or actual maintenance resource use.

#### Request

```http
POST /services/pulse/food-beverage/parts-and-labour/insert
Content-Type: application/json
```

```json
[
  {
    "workOrder": "WO-8830",
    "category": "labour",
    "item": "CREW-MAINT",
    "plannedQuantity": 3,
    "actualQuantity": 3.4,
    "unit": "h",
    "unitValue": 31,
    "at": "2026-08-14T11:20:00+02:00"
  },
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
]
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `workOrder` | string | yes | Business code of the Work order. |
| `category` | string | yes | Resource category. |
| `item` | string | yes | Business code of the resource. |
| `lot` | string or null | no | Lot code when a traceable material lot was used. |
| `plannedQuantity` | number or null | no | Planned resource quantity. |
| `actualQuantity` | number or null | no | Actual resource quantity. |
| `unit` | string | yes | Unit of the quantities. |
| `unitValue` | number or null | no | Cost per unit. |
| `at` | string or null | no | Date and time associated with actual use. |