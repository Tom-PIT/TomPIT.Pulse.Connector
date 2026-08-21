# Clean regime

Represents a cleaning regime used in Food & Beverage operations.

A clean regime defines the kind of cleaning performed, such as a dry clean, wet clean, full clean-in-place (CIP), or allergen clean.

## The Clean regime object

```json
{
  "code": "FULL-CIP",
  "name": "Full clean-in-place",
  "types": {
    "cleanType": "CIP"
  },
  "attributes": {
    "procedureCode": "CIP-STD-01"
  }
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the clean regime in source systems and integrations. | `"FULL-CIP"` |
| `name` | string | Human-readable name of the clean regime. | `"Full clean-in-place"` |
| `types` | object or null | Optional classifications used to group and analyse the clean regime. | `{ "cleanType": "CIP" }` |
| `attributes` | object or null | Optional additional source-system metadata associated with the clean regime. These values are stored but are not used for analysis. | `{ "procedureCode": "CIP-STD-01" }` |

</div>

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## API resource

| Resource | Base path |
| --- | --- |
| Clean regime | `/services/pulse/food-beverage/clean-regimes` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.
