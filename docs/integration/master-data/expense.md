# Expense

Represents an additional type of cost tracked in Pulse.

## The Expense object

```json
{
  "id": 27,
  "code": "TRANSPORT",
  "name": "Transport cost",
  "price": 45.00,
  "description": "Additional transport-related cost"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `27` |
| `code` | string | Business code used to identify the expense in external systems and integrations. | `"TRANSPORT"` |
| `name` | string | Human-readable name of the expense. | `"Transport cost"` |
| `price` | number or null | Optional default price. Pulse may use this value when a related operational record does not provide its own price. | `45.00` |
| `description` | string or null | Optional description of the expense and when it is used. | `"Additional transport-related cost"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `ExpenseService` | `/services/pulse/types/expenses` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Used by

Expenses are referenced by:

- [Expense plans](../manufacturing/expense-plan.md)
- [Expense usage records](../manufacturing/expense-usage.md)
- [Waste expense usage records](../manufacturing/waste-expense-usage.md)