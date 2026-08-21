# Customer

Represents a customer associated with Food & Beverage operations in Pulse.

## The Customer object

```json
{
  "code": "CUST-001",
  "name": "Example Customer",
  "taxNumber": "SI12345678",
  "group": "CG-RETAIL",
  "types": {
    "channel": "RETAIL"
  },
  "attributes": {
    "country": "SI"
  }
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the customer in source systems and integrations. | `"CUST-001"` |
| `name` | string | Human-readable name of the customer. | `"Example Customer"` |
| `taxNumber` | string or null | Optional tax number used to identify the customer across source systems. | `"SI12345678"` |
| `group` | string or null | Optional code identifying the customer group. | `"CG-RETAIL"` |
| `types` | object or null | Optional classifications used to group and analyse the customer. | `{ "channel": "RETAIL" }` |
| `attributes` | object or null | Optional additional source-system metadata associated with the customer. These values are stored but are not used for analysis. | `{ "country": "SI" }` |

</div>

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## API resource

| Resource | Base path |
| --- | --- |
| Customer | `/services/pulse/food-beverage/customers` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.