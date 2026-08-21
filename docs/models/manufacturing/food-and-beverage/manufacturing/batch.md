# Batch

Represents a process batch such as a cook, mix, fermentation, or other bulk-production step.

A batch describes the process side of Food & Beverage production. It is typically associated with a vessel and recipe and produces a bulk lot that can later supply one or more production runs.

## The Batch object

```json
{
  "code": "BULK-260810-07",
  "vessel": "TANK-3",
  "recipe": "REC-BASE-v2",
  "produces": "BULK-260810-07",
  "at": "2026-08-10T18:00:00+02:00"
}
```

When the batch finishes, submit the values that became known:

```json
{
  "code": "BULK-260810-07",
  "end": "2026-08-10T21:30:00+02:00",
  "quantity": 4200,
  "unit": "kg",
  "expiresAt": "2026-08-17T00:00:00+02:00"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the process batch in source systems and integrations. | `"BULK-260810-07"` |
| [`vessel`](../master-data/vessel.md) | string | Code of the vessel in which the batch is processed. | `"TANK-3"` |
| [`recipe`](../master-data/recipe.md) | string | Code of the recipe or formulation used for the batch. | `"REC-BASE-v2"` |
| [`produces`](../master-data/lot.md) | string | Code of the bulk lot produced by the batch. | `"BULK-260810-07"` |
| `at` | string | Timestamp when the batch started, in ISO 8601 format with an explicit offset. | `"2026-08-10T18:00:00+02:00"` |
| `end` | string or null | Timestamp when the batch finished, in ISO 8601 format with an explicit offset. | `"2026-08-10T21:30:00+02:00"` |
| `quantity` | number or null | Quantity of bulk material produced by the batch. | `4200` |
| `unit` | string or null | Unit in which the produced quantity is expressed. | `"kg"` |
| `expiresAt` | string or null | Optional expiry timestamp for the produced bulk lot. | `"2026-08-17T00:00:00+02:00"` |

</div>

## Batch lifecycle

A batch can be submitted when processing starts:

```json
{
  "code": "BULK-260810-07",
  "vessel": "TANK-3",
  "recipe": "REC-BASE-v2",
  "produces": "BULK-260810-07",
  "at": "2026-08-10T18:00:00+02:00"
}
```

When processing finishes, submit the same `code` with the fields that became known:

```json
{
  "code": "BULK-260810-07",
  "end": "2026-08-10T21:30:00+02:00",
  "quantity": 4200,
  "unit": "kg"
}
```

Fields omitted from the second request remain unchanged.

The produced quantity describes the bulk output of the process batch.

## Batch and run

A batch and a [run](run.md) represent different parts of production.

```text
Process
  Batch
    ↓
  Bulk lot
    ↓
Packing
  Run
```

A batch represents process production such as mixing, cooking, or fermentation.

A run represents production of a specific product on a production line.

The relationship is not necessarily one-to-one. One batch can supply several runs, and one run can draw from several batches.

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

This preserves the connection between the process conditions that produced the bulk material and the production runs that later consumed it.

## Produced lot

The value in `produces` identifies the bulk [lot](../master-data/lot.md) created by the batch.

The bulk lot can carry measured composition through lot analysis, for example fat, protein, solids, or Brix.

```text
POST /services/pulse/food-beverage/lots/{code}/analysis
```

When a later run references that bulk batch, Pulse can preserve the composition and genealogy context from the process side of production.

## Consumption

Materials consumed while producing the batch are recorded through [consumption](consumption.md).

For example:

```json
{
  "batch": "BULK-260810-07",
  "category": "ingredient",
  "item": "MILK-RAW",
  "lot": "MILK-2026-0717-A",
  "quantity": 940,
  "unit": "kg",
  "unitValue": 0.68,
  "at": "2026-08-10T18:20:00+02:00"
}
```

This preserves the genealogy from incoming material lots through the process batch and its produced bulk lot.

## Expiry

`expiresAt` can be supplied when the produced bulk lot has a known expiry time.

When it is not supplied, Pulse may derive the expiry from an applicable declared shelf-life expectation. When neither is available, the bulk lot has no declared expiry.

## API resource

| Resource | Base path |
| --- | --- |
| Batch | `/services/pulse/food-beverage/batches` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Vessel](../master-data/vessel.md)
- [Recipe](../master-data/recipe.md)
- [Lot](../master-data/lot.md)

The referenced records must be available before submitting the batch.