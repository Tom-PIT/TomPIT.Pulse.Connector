# Maintenance expense usage

Records an actual additional expense for a maintenance activity.

## The Maintenance expense usage object

```json
{
  "id": 601,
  "maintenance": 314,
  "expense": 15,
  "quantity": 1.0,
  "price": 135.0,
  "date": "2026-07-20T09:15:00Z"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `601` |
| [`maintenance`](maintenance.md) | integer | Pulse `id` of the maintenance activity. | `314` |
| [`expense`](../master-data/expense.md) | integer | Pulse `id` of the expense used for the maintenance activity. | `15` |
| `quantity` | number | Actual actual quantity or scope of the expense. | `1.0` |
| `price` | number | Optional actual price per expense unit. | `135.0` |
| `date` | string | Date and time when the usage occurred or was recorded. | `"2026-07-20T09:15:00Z"` |

</div>

## API resource

| Service | Base path |
| --- | --- |
| `MaintenanceExpenseUsageService` | `/services/pulse/maintenance/usage/expenses` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Maintenance](maintenance.md)
- [Expense](../master-data/expense.md)

Create or retrieve the applicable records before submitting the maintenance expense usage.
