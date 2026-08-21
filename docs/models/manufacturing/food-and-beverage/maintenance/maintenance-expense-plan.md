# Maintenance expense plan

Describes planned additional expense for a maintenance activity.

## The Maintenance expense plan object

```json
{
  "id": 501,
  "maintenance": 314,
  "expense": 15,
  "quantity": 1.0,
  "price": 120.0
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `501` |
| [`maintenance`](maintenance.md) | integer | Pulse `id` of the maintenance activity. | `314` |
| [`expense`](../master-data/expense.md) | integer | Pulse `id` of the expense planned for the maintenance activity. | `15` |
| `quantity` | number | Planned planned quantity or scope of the expense. | `1.0` |
| `price` | number | Optional planned price per expense unit. | `120.0` |

</div>

## API resource

| Service | Base path |
| --- | --- |
| `MaintenanceExpensePlanService` | `/services/pulse/maintenance/plan/expenses` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Maintenance](maintenance.md)
- [Expense](../master-data/expense.md)

Create or retrieve the applicable records before submitting the maintenance expense plan.
