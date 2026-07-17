# Customer

Represents a customer associated with business or operational activities in Pulse.

## The Customer object

```json
{
  "id": 25,
  "code": "CUST-001",
  "name": "Example customer"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `25` |
| `code` | string | Business code used to identify the customer in external systems and integrations. | `"CUST-001"` |
| `name` | string | Human-readable name of the customer. | `"Example customer"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `CustomerService` | `/services/pulse/types/customers` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Referenced by

- [Batches](../manufacturing/batch.md)