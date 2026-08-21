# Plant

Represents a physical operating location in Pulse.

## The Plant object

```json
{
  "code": "PLANT-LJ",
  "name": "Ljubljana plant",
  "types": {
    "region": "CENTRAL-EUROPE"
  },
  "attributes": {
    "erpCode": "1000"
  }
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the plant in external systems and integrations. | `"PLANT-LJ"` |
| `name` | string | Human-readable name of the plant. | `"Ljubljana plant"` |
| `types` | object or null | Optional classifications used to group and analyse the plant. | `{ "region": "CENTRAL-EUROPE" }` |
| `attributes` | object or null | Optional additional source-system attributes associated with the plant. These values are stored with the plant but are not used for analysis. | `{ "erpCode": "1000" }` |

</div>

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## API resource

| Resource | Base path |
| --- | --- |
| `Plant` | `/services/pulse/food-beverage/plants` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.