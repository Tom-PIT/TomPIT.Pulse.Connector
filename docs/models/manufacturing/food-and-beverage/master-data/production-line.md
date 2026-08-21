# Production line

Represents a production line within a plant.

## The Production line object

```json
{
  "code": "YOGURT-LINE-01",
  "name": "Yogurt Filling Line 1",
  "plant": "PLANT-LJ",
  "types": {
    "format": "CUP-FILLING"
  },
  "attributes": {
    "installed": 2019
  }
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the production line in source systems and integrations. | `"YOGURT-LINE-01"` |
| `name` | string | Human-readable name of the production line. | `"Yogurt Filling Line 1"` |
| `plant` | string | Code of the plant to which the production line belongs. | `"PLANT-LJ"` |
| `types` | object or null | Optional classifications used to group and analyse the production line. | `{ "format": "CUP-FILLING" }` |
| `attributes` | object or null | Optional additional source-system metadata associated with the production line. These values are stored but are not used for analysis. | `{ "installed": 2019 }` |

</div>

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## API resource

| Resource | Base path |
| --- | --- |
| Production line | `/services/pulse/food-beverage/lines` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Plant](plant.md)

The plant referenced by `plant` must be available before submitting the production line.