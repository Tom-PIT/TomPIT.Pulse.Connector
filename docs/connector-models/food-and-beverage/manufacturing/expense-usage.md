# Expense usage

Represents an additional cost recorded during a stage.

Expense usage covers costs that are not represented as material, labor, energy, or equipment use. Examples include external services, additional inspections, cleaning, transport, laboratory work, documentation, sorting, or another cost incurred during execution.

Pulse can compare the actual expense with the related [expense plan](expense-plan.md).

## The Expense usage object

```json
{
  "id": 756,
  "stage": 208,
  "expense": 29,
  "quantity": 1,
  "price": 180.0,
  "date": "2026-07-20T10:48:00+02:00"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier of the actual expense record. | `756` |
| [`stage`](stage.md) | integer | Pulse `id` of the stage to which the expense applies. | `208` |
| [`expense`](../master-data/expense.md) | integer | Pulse `id` of the expense that was recorded. | `29` |
| `quantity` | number | Actual quantity or scope of the expense. | `1` |
| `price` | number or null | Optional actual price or value for this expense record. | `180.0` |
| `date` | string | Date and time when the expense was recorded or incurred, in ISO 8601 format. | `"2026-07-20T10:48:00+02:00"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `ExpenseUsageService` | `/services/pulse/manufacturing/batches/stages/usage/expenses` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Stage](stage.md)
- [Expense](../master-data/expense.md)

Create or retrieve the applicable records before submitting the expense usage record.