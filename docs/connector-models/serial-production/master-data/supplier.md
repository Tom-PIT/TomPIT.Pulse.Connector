# Supplier

Represents a supplier associated with material or energy usage in Pulse.

## The Supplier object

```json
{
  "id": 36,
  "code": "SUP-001",
  "name": "Example supplier",
  "description": "Supplier of raw materials and production consumables"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `36` |
| `code` | string | Business code used to identify the supplier in external systems and integrations. | `"SUP-001"` |
| `name` | string | Human-readable name of the supplier. | `"Example supplier"` |
| `description` | string or null | Optional description of the supplier and the goods or services it provides. | `"Supplier of raw materials and production consumables"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `SupplierService` | `/services/pulse/types/suppliers` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Referenced by

- [Material usage records](../manufacturing/material-usage.md)
- [Energy source usage records](../manufacturing/energy-source-usage.md)
- [Waste material usage records](../manufacturing/waste-material-usage.md)
- [Waste energy source usage records](../manufacturing/waste-energy-source-usage.md)