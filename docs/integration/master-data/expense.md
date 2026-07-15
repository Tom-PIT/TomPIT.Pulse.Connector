# Expense

Represents an additional type of cost tracked in Pulse.

## The Expense object

```json
{
  "id": 27,
  "code": "TRANSPORT",
  "name": "Transport cost",
  "price": 45.00
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `27` |
| `code` | string | Business code used to identify the expense in external systems and integrations. | `"TRANSPORT"` |
| `name` | string | Human-readable name of the expense. | `"Transport cost"` |
| `price` | number or null | Default price per unit used as the baseline for expense-related calculations. | `45.00` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `ExpenseService` | `/services/pulse/types/expenses` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Used by

Expenses are referenced by:

- Expense plans
- Expense usage records
- Waste expense usage records
