# Supplier

Represents a supplier associated with materials and other inputs used in Food & Beverage operations.

## The Supplier object

```json
{
  "code": "SUP-001",
  "name": "Alpine Milk Cooperative Slovenia",
  "taxNumber": "SI12345678",
  "group": "SG-COOP",
  "types": {
    "supplierType": "COOPERATIVE"
  },
  "attributes": {
    "country": "SI",
    "approvalExpires": "2027-03-31"
  }
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the supplier in source systems and integrations. | `"SUP-001"` |
| `name` | string | Human-readable name of the supplier. | `"Alpine Milk Cooperative Slovenia"` |
| `taxNumber` | string or null | Optional tax number used to identify the supplier across source systems. | `"SI12345678"` |
| `group` | string or null | Optional code identifying the supplier group. | `"SG-COOP"` |
| `types` | object or null | Optional classifications used to group and analyse the supplier. | `{ "supplierType": "COOPERATIVE" }` |
| `attributes` | object or null | Optional additional source-system metadata associated with the supplier. These values are stored but are not used for analysis. | `{ "country": "SI", "approvalExpires": "2027-03-31" }` |

</div>

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## API resource

| Resource | Base path |
| --- | --- |
| Supplier | `/services/pulse/food-beverage/suppliers` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.