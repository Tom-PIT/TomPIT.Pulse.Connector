# Material

Represents an ingredient, packaging material, chemical, or other material used in Food & Beverage operations.

## The Material object

```json
{
  "code": "MILK-RAW",
  "name": "Raw Milk 3.8% Fat",
  "measureUnit": "kg",
  "group": "MG-DAIRY-RAW",
  "lotTracked": true,
  "types": {
    "allergen": "ALG-MILK",
    "storage": "CHILLED",
    "origin": "SI"
  },
  "attributes": {
    "supplierPartNo": "RM-3801"
  }
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the material in source systems and integrations. | `"MILK-RAW"` |
| `name` | string | Human-readable name of the material. | `"Raw Milk 3.8% Fat"` |
| `measureUnit` | string | Code of the measure unit in which material quantities are expressed. | `"kg"` |
| `group` | string or null | Optional code identifying the material group. | `"MG-DAIRY-RAW"` |
| `lotTracked` | boolean | Indicates whether consumption of the material must reference a lot. Defaults to `true`. | `true` |
| `types` | object or null | Optional classifications used to group and analyse the material, such as allergen, storage, or origin. | `{ "allergen": "ALG-MILK", "storage": "CHILLED" }` |
| `attributes` | object or null | Optional additional source-system attributes. These values are stored with the material but are not used for analysis. | `{ "supplierPartNo": "RM-3801" }` |

</div>

## Lot tracking

Materials that require traceability should use `lotTracked: true`.

When `lotTracked` is `true`, consumption records for the material must reference a lot. Materials that are not normally lot-tracked, such as water, steam, or similar utilities, can use `lotTracked: false`.

## API resource

| Resource | Base path |
| --- | --- |
| Material | `/services/pulse/food-beverage/materials` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Measure unit](measure-unit.md)

The measure unit referenced by `measureUnit` must be available before submitting the material.

## Referenced by

- [Material plans](../manufacturing/material-plan.md)
- [Material usage records](../manufacturing/material-usage.md)
- [Waste material usage records](../manufacturing/waste-material-usage.md)