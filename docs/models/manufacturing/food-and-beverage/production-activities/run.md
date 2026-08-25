# Run

Represents a production episode for a product on a production line.

A run is the main unit of production analysis in the Food & Beverage model. Output, consumption, process measurements, line states, events, and stages can all contribute context to the run.

## The Run object

```json
{
  "code": "L03-260810-002",
  "line": "L03",
  "product": "SKU-4471",
  "recipe": "REC-4471-v3",
  "shift": "night",
  "crew": "CREW-C",
  "from": [
    "BULK-260810-07"
  ],
  "at": "2026-08-10T22:00:00+02:00",
  "declaredWeight": 500,
  "declaredUnit": "g",
  "unitPrice": 1.34,
  "plan": {
    "quantity": 24000,
    "unit": "pcs",
    "items": [
      {
        "category": "ingredient",
        "item": "MAT0042",
        "quantity": 300,
        "unit": "kg",
        "unitValue": 2.44
      },
      {
        "category": "packaging",
        "item": "PKG0001",
        "quantity": 24000,
        "unit": "pcs",
        "unitValue": 0.031
      }
    ]
  }
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the run in source systems and integrations. | `"L03-260810-002"` |
| [`line`](../master-data/production-line.md) | string | Code of the production line on which the run takes place. | `"L03"` |
| [`product`](../master-data/product.md) | string | Code of the product produced during the run. | `"SKU-4471"` |
| [`recipe`](../master-data/recipe.md) | string or null | Optional code of the recipe or formulation version used for the run. | `"REC-4471-v3"` |
| [`shift`](../master-data/shift.md) | string or null | Optional code of the shift associated with the start of the run. | `"night"` |
| [`crew`](../master-data/crew.md) | string or null | Optional code of the crew associated with the run. | `"CREW-C"` |
| [`from`](batch.md) | array of strings or null | Optional codes of process batches that supplied bulk material to the run. | `["BULK-260810-07"]` |
| `at` | string | Timestamp when the run started, in ISO 8601 format with an explicit offset. | `"2026-08-10T22:00:00+02:00"` |
| `end` | string or null | Timestamp when the run ended, in ISO 8601 format with an explicit offset. | `"2026-08-11T06:00:00+02:00"` |
| `status` | string or null | Current run status. Use `completed` or `cancelled` when closing the run. | `"completed"` |
| `declaredWeight` | number or null | Declared product weight applicable to the run. | `500` |
| `declaredUnit` | string or null | Unit in which `declaredWeight` is expressed. | `"g"` |
| `unitPrice` | number or null | Product unit value applicable to the run. | `1.34` |
| `plan` | object or null | Optional production plan containing the planned output quantity and planned resource items. | `{ "quantity": 24000, "unit": "pcs" }` |

</div>

## Run lifecycle

A run does not need to be submitted only after production has finished.

When production starts, submit the information that is already known:

```json
{
  "code": "L03-260810-002",
  "line": "L03",
  "product": "SKU-4471",
  "at": "2026-08-10T22:00:00+02:00"
}
```

When the run finishes, submit the same `code` with the fields that became known:

```json
{
  "code": "L03-260810-002",
  "end": "2026-08-11T06:00:00+02:00",
  "status": "completed"
}
```

Fields omitted from the second request remain unchanged.

A run that started but was stopped before normal completion can use:

```json
{
  "code": "L03-260810-002",
  "end": "2026-08-11T01:20:00+02:00",
  "status": "cancelled"
}
```

A cancelled run is different from a retracted run. Use `cancelled` when production actually started but did not complete. Use `DELETE` when the run record should not exist.

## Production context

A run connects the commercial and production context for one production episode.

```text
Production line
      │
      └── Run
          ├── Product
          ├── Recipe
          ├── Shift
          ├── Crew
          └── Source process batches
```

`recipe`, `shift`, and `crew` allow Pulse to compare otherwise similar runs under different production conditions.

When a run spans more than one shift, `shift` identifies the shift in which the run started.

## Source batches

The optional `from` field identifies process batches that supplied bulk product to the run.

For example:

```json
{
  "code": "L03-260810-002",
  "from": [
    "BULK-260810-07",
    "BULK-260810-08"
  ]
}
```

A process batch and a production run are separate concepts. One process batch can supply multiple runs, and one run can draw from multiple process batches.

Keeping this relationship allows production results to be traced back to the process conditions and material composition of the bulk batches that supplied the run.

## Production plan

The production plan can be included when the run is submitted:

```json
{
  "plan": {
    "quantity": 24000,
    "unit": "pcs",
    "items": [
      {
        "category": "ingredient",
        "item": "MAT0042",
        "quantity": 300,
        "unit": "kg",
        "unitValue": 2.44
      }
    ]
  }
}
```

Each planned item identifies the specific resource being planned. This allows planned quantities and values to be compared with actual [consumption](consumption.md).

If the plan becomes available after the run has already been submitted, it can also be submitted separately through:

`POST /services/pulse/food-beverage/runs/{code}/plan`

## API resource

| Resource | Base path |
| --- | --- |
| Run | `/services/pulse/food-beverage/runs` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Production line](../master-data/production-line.md)
- [Product](../master-data/product.md)
- [Recipe](../master-data/recipe.md), when provided
- [Shift](../master-data/shift.md), when provided
- [Crew](../master-data/crew.md), when provided
- [Batch](batch.md), when `from` is provided

Referenced records must be available before submitting the run.