# Vessel

Represents a tank, silo, or other process vessel associated with a production line.

## The Vessel object

```json
{
  "code": "TANK-03",
  "name": "Fermentation Tank 3",
  "line": "YOGURT-LINE-01",
  "types": {
    "vesselType": "FERMENTATION"
  },
  "attributes": {
    "capacity": 5000,
    "material": "stainless-steel"
  }
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the vessel in source systems and integrations. | `"TANK-03"` |
| `name` | string | Human-readable name of the vessel. | `"Fermentation Tank 3"` |
| [`line`](production-line.md) | string | Code of the production line to which the vessel belongs. | `"YOGURT-LINE-01"` |
| `types` | object or null | Optional classifications used to group and analyse the vessel. | `{ "vesselType": "FERMENTATION" }` |
| `attributes` | object or null | Optional additional source-system metadata associated with the vessel. These values are stored but are not used for analysis. | `{ "capacity": 5000, "material": "stainless-steel" }` |

</div>

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## API resource

| Resource | Base path |
| --- | --- |
| Vessel | `/services/pulse/food-beverage/vessels` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Production line](production-line.md)

The production line referenced by `line` must be available before submitting the vessel.