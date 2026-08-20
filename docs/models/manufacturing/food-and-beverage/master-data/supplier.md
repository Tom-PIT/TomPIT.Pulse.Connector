# Supplier

Represents a supplier associated with materials and other inputs used in Food & Beverage operations.

## The Supplier object

```json
{
  "code": "SUP-001",
  "name": "Alpine Milk Cooperative Slovenia",
  "taxNumber": "SI12345678",
  "group": "SG-COOP",
  "attributes": {
    "country": "SI",
    "approvalExpires": "2027-03-31"
  }
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the supplier in source systems and integrations. | `"SUP-001"` |
| `name` | string | Human-readable name of the supplier. | `"Alpine Milk Cooperative Slovenia"` |
| `taxNumber` | string or null | Optional tax number used to identify the supplier across source systems. | `"SI12345678"` |
| `group` | string or null | Optional code identifying the supplier group. | `"SG-COOP"` |
| `attributes` | object or null | Optional additional source-system attributes associated with the supplier. | `{ "country": "SI", "approvalExpires": "2027-03-31" }` |

</div>

## API resource

| Resource | Base path |
| --- | --- |
| Supplier | `/services/pulse/food-beverage/suppliers` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Referenced by

- [Material usage records](../manufacturing/material-usage.md)
- [Energy source usage records](../manufacturing/energy-source-usage.md)
- [Waste material usage records](../manufacturing/waste-material-usage.md)
- [Waste energy source usage records](../manufacturing/waste-energy-source-usage.md)