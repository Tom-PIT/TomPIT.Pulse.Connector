# Expense plan

Represents an additional cost planned for a stage.

Expense plans cover costs that are not represented as material, labor, energy, or equipment use. Examples include external services, inspections, cleaning, transport, documentation, or other stage-related costs.

Pulse can compare the planned expense with the related [expense usage](expense-usage.md).

## The Expense plan object

```json
{
  "id": 534,
  "stage": 208,
  "expense": 29,
  "quantity": 1,
  "price": 150.0
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier of the planned expense record. | `534` |
| [`stage`](stage.md) | integer | Pulse `id` of the stage to which the planned expense applies. | `208` |
| [`expense`](../master-data/expense.md) | integer | Pulse `id` of the expense planned for the stage. | `29` |
| `quantity` | number | Planned quantity or scope of the expense. | `1` |
| `price` | number or null | Optional planned price or value of the expense. | `150.0` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `ExpensePlanService` | `/services/pulse/manufacturing/batches/stages/plan/expenses` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Stage](stage.md)
- [Expense](../master-data/expense.md)

Create or retrieve the applicable records before submitting the expense plan.