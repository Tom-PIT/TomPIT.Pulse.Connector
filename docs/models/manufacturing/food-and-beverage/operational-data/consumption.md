# Consumption

<!-- TODO: This page is currently based on ApiSurfaceRevised. Revisit it once the implementation is available and verify fields, routes, query parameters, batching behavior, and examples against the current code. -->

Records resources actually used by a Run, Batch, or Clean.

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
| `usedBy` | string | Business code of the [Run](../production-activities/run.md), [Batch](../production-activities/batch.md), or [Clean](../production-activities/clean.md) that used the resource. | `"BULK-260810-07"` |
| `category` | string | Resource category. See the supported categories below. | `"ingredient"` |
| `item` | string | Business code of the specific Material, Cost line, Crew, or Machine that was used. | `"MAT0001"` |
| [`lot`](../production-activities/lot.md) | string or null | Optional lot code identifying the specific material quantity consumed. Required when the material is lot-tracked. | `"IN-260807-441"` |
| `quantity` | number | Quantity actually consumed. | `940` |
| `unit` | string | Unit in which the consumed quantity is expressed. | `"kg"` |
| `unitValue` | number or null | Optional cost per unit at the time the resource was used. | `3.12` |
| `at` | string or null | Optional approximate date and time of the consumption. | `"2026-08-10T18:20:00+02:00"` |

</div>

> [!IMPORTANT]
> `usedBy`, `item`, `lot`, and `at` together identify a consumption record.
>
> When the referenced material uses `lotTracked: true`, `lot` is required.

## Categories

Consumption uses the same resource categories as [Planned use](../production-activities/planned-use.md).

| Category | `item` identifies |
| --- | --- |
| `ingredient` | A [Material](../master-data/material.md) used as an ingredient. |
| `packaging` | A [Material](../master-data/material.md) used for packaging. |
| `chemical` | A [Material](../master-data/material.md) used as a chemical. |
| `water` | A water resource. |
| `energy` | An energy resource. |
| `effluent` | An effluent resource. |
| `labour` | A [Crew](../master-data/crew.md). |
| `equipment` | A [Machine](../master-data/machine.md). |
| `expense` | A [Cost line](../master-data/cost-line.md). |

The category identifies the kind of cost or resource, while `item` identifies the specific resource that was actually used.

## Lot traceability

For lot-tracked materials, include the specific lot that was consumed:

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

This preserves genealogy from the incoming lot to the production activity that consumed it.

If analysis values were recorded for that lot, those properties can remain associated with the material as it moves through production.

For materials that are not lot-tracked, `lot` can be omitted.

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

Because both records identify the same category and item, Pulse can compare planned and actual resource use.

## Labor consumption

Labor is recorded at Crew level rather than for individual people.

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

The API category value remains `labour`.

## Cleaning consumption

A Clean can have its own consumption records.

For example:

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

This allows the actual cost of cleaning to be compared with the production changeover that required it.

## Batching

The specification shows Consumption accepting multiple records in one request:

```json
[
  {
    "usedBy": "BULK-260810-07",
    "category": "ingredient",
    "item": "MAT0001",
    "lot": "IN-260807-441",
    "quantity": 940,
    "unit": "kg",
    "unitValue": 3.12,
    "at": "2026-08-10T18:20:00+02:00"
  },
  {
    "usedBy": "L01-260810-002",
    "category": "labour",
    "item": "CREW-C",
    "quantity": 32,
    "unit": "h",
    "unitValue": 24.5,
    "at": "2026-08-11T05:50:00+02:00"
  }
]
```

The exact batching behavior should be verified once the implementation is available.

## API resource

| Resource | Base path |
| --- | --- |
| `Consumption` | `/services/pulse/food-beverage/consumption` |

## API methods

> [!NOTE]
> The API methods below are provisional until the Consumption implementation is available for verification.

### Submit consumption

`POST /services/pulse/food-beverage/consumption/insert`

Records actual resource consumption.

#### Request

```http
POST /services/pulse/food-beverage/consumption/insert
Content-Type: application/json
```

```json
[
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
]
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `usedBy` | string | yes | Business code of the run, batch, or clean. |
| `category` | string | yes | Resource category. |
| `item` | string | yes | Business code of the resource consumed. |
| `lot` | string or null | conditional | Required when the referenced material is lot-tracked. |
| `quantity` | number | yes | Quantity actually consumed. |
| `unit` | string | yes | Unit of the consumed quantity. |
| `unitValue` | number or null | no | Cost per unit at the time of use. |
| `at` | string or null | no | Approximate date and time of consumption. |