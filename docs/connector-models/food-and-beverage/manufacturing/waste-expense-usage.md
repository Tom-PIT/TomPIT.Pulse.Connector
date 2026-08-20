# Waste expense usage

Represents an additional expense attributed to a waste record.

Use this record for costs caused by waste that are not represented as material or energy consumption, such as inspection, disposal, transport, cleaning, sorting, external services, or documentation.

## The Waste expense usage object

```json
{
  "id": 815,
  "waste": 812,
  "quantity": 1,
  "expense": 29,
  "price": 180.0
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier of the waste expense usage record. | `815` |
| [`waste`](waste.md) | integer | Pulse `id` of the waste record to which the expense is attributed. | `812` |
| `quantity` | number | Quantity or scope of the expense attributed to the waste. | `1` |
| [`expense`](../master-data/expense.md) | integer | Pulse `id` of the expense associated with the waste. | `29` |
| `price` | number or null | Optional price or value of the expense for this record. | `180.0` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `WasteExpenseUsageService` | `/services/pulse/manufacturing/batches/stages/usage/waste/expenses` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Waste](waste.md)
- [Expense](../master-data/expense.md)

Create or retrieve the applicable records before submitting the waste expense usage record.
