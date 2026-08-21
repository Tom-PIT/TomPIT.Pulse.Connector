# Consumption

Represents a resource consumed while production, cleaning, or maintenance work is performed.

Consumption records actual quantities and values for ingredients, packaging, chemicals, utilities, labor, equipment, and other costs through one common resource.

## The Consumption object

```json
{
  "run": "L03-260810-002",
  "category": "ingredient",
  "item": "MILK-RAW",
  "lot": "IN-260807-441",
  "quantity": 240,
  "unit": "kg",
  "unitValue": 0.68,
  "at": "2026-08-10T22:30:00+02:00"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| [`run`](run.md) | string | Code of the production run that consumed the resource. Exactly one consumption subject must be provided. | `"L03-260810-002"` |
| [`batch`](batch.md) | string | Code of the process batch that consumed the resource. Exactly one consumption subject must be provided. | `"BULK-260810-07"` |
| [`clean`](clean.md) | string | Code of the cleaning activity that consumed the resource. Exactly one consumption subject must be provided. | `"CIP-260811-014"` |
| [`maintenance`](../maintenance/maintenance.md) | string | Code of the maintenance activity that consumed the resource. Exactly one consumption subject must be provided. | `"WO-260811-17"` |
| `category` | string | Category of resource consumed. | `"ingredient"` |
| `item` | string | Code of the specific resource consumed. Its meaning depends on `category`. | `"MILK-RAW"` |
| [`lot`](../master-data/lot.md) | string or null | Optional lot code identifying the specific material quantity consumed. Required when the referenced material is lot-tracked. | `"IN-260807-441"` |
| `quantity` | number | Quantity consumed. | `240` |
| `unit` | string | Unit in which the consumed quantity is expressed. | `"kg"` |
| `unitValue` | number or null | Optional value per unit at the time of consumption. | `0.68` |
| `at` | string | Timestamp when the consumption was recorded, in ISO 8601 format with an explicit offset. | `"2026-08-10T22:30:00+02:00"` |

</div>

Exactly one of `run`, `batch`, `clean`, or `maintenance` identifies the work to which the consumption belongs.

## Categories

The `category` determines what type of resource `item` identifies.

| Category | `item` identifies | Example |
| --- | --- | --- |
| `ingredient` | A [material](../master-data/material.md) used as an ingredient. | `"MILK-RAW"` |
| `packaging` | A [material](../master-data/material.md) used for packaging. | `"CUP-150G"` |
| `chemical` | A [material](../master-data/material.md) such as a cleaning chemical. | `"CAUSTIC-01"` |
| `energy` | An energy source or utility. | `"ELECTRICITY"` |
| `water` | A water source or utility. | `"MAINS-WATER"` |
| `effluent` | An effluent stream or utility. | `"EFFLUENT"` |
| `labour` | A [crew](../master-data/crew.md), never an individual person. | `"CREW-C"` |
| `equipment` | A [machine](../master-data/machine.md). | `"L03-FILLER"` |
| `expense` | A named cost line. | `"LAB-ANALYSIS"` |

The category is intentionally broad while `item` identifies the specific resource.

For example, these are two separate ingredient consumptions:

```json
[
  {
    "run": "L03-260810-002",
    "category": "ingredient",
    "item": "MILK-RAW",
    "quantity": 940,
    "unit": "kg",
    "at": "2026-08-10T22:30:00+02:00"
  },
  {
    "run": "L03-260810-002",
    "category": "ingredient",
    "item": "CULTURE-ST01",
    "quantity": 120,
    "unit": "g",
    "at": "2026-08-10T22:31:00+02:00"
  }
]
```

Keeping the item on every record is important because resources within the same category can use different units and have different planned quantities and costs.

## Lot traceability

When material consumption references a lot, Pulse can preserve the connection between the work and the exact material quantity that was used.

```json
{
  "run": "L03-260810-002",
  "category": "ingredient",
  "item": "MILK-RAW",
  "lot": "IN-260807-441",
  "quantity": 940,
  "unit": "kg",
  "at": "2026-08-10T22:30:00+02:00"
}
```

If the material uses `lotTracked: true`, the consumption record must include `lot`.

Materials that are not lot-tracked, such as some utilities, can be consumed without a lot reference.

When analysis values such as fat, protein, or moisture have been recorded for the consumed lot, that composition can contribute context to analysis of the consuming production work.

## Planned and actual consumption

Planned resource quantities are defined in the production plan, while `/consumption` records what was actually used.

For example, a run may plan:

```json
{
  "category": "ingredient",
  "item": "MILK-RAW",
  "quantity": 1000,
  "unit": "kg",
  "unitValue": 0.68
}
```

and actual consumption may later record:

```json
{
  "run": "L03-260810-002",
  "category": "ingredient",
  "item": "MILK-RAW",
  "quantity": 1040,
  "unit": "kg",
  "unitValue": 0.68,
  "at": "2026-08-10T22:30:00+02:00"
}
```

Because both records identify the same `category` and `item`, Pulse can compare planned and actual resource use.

## Labor consumption

Labor is recorded at crew level.

```json
{
  "run": "L03-260810-002",
  "category": "labour",
  "item": "CREW-C",
  "quantity": 32,
  "unit": "h",
  "unitValue": 24.5,
  "at": "2026-08-11T06:00:00+02:00"
}
```

Pulse does not require or expose individual people for labor consumption. The `item` identifies a registered [crew](../master-data/crew.md).

## Cleaning consumption

The same resource can record what a cleaning activity consumed:

```json
{
  "clean": "CIP-260811-014",
  "category": "chemical",
  "item": "CAUSTIC-01",
  "quantity": 40,
  "unit": "l",
  "unitValue": 0.62,
  "at": "2026-08-11T06:20:00+02:00"
}
```

Water, chemicals, labor, energy, and other cleaning costs therefore use the same consumption model as production work.

## Batching

The consumption resource accepts either one object or an array of objects.

This allows individual records to be submitted as they occur or multiple captures to be sent together by a source system.

## API resource

| Resource | Base path |
| --- | --- |
| Consumption | `/services/pulse/food-beverage/consumption` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

Consumption must reference exactly one work record:

- [Run](run.md)
- [Batch](batch.md)
- [Clean](clean.md)
- [Maintenance](../maintenance/maintenance.md)

Depending on `category`, the record may also reference:

- [Material](../master-data/material.md)
- [Lot](../master-data/lot.md), when the material is lot-tracked
- [Crew](../master-data/crew.md) for labor
- [Machine](../master-data/machine.md) for equipment

Referenced records must be available before submitting the consumption record.