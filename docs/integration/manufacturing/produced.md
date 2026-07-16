# Produced

Represents an output quantity recorded for a batch.

Produced records include both good and bad output. A product classified as bad quality is still recorded as produced because it was completed as an output of the batch.

> [!IMPORTANT]
> In Pulse, **bad-quality output is not the same as waste**. A bad produced item remains part of the recorded output and is distinguished through its quality classification.

## The Produced object

```json
{
  "id": 415,
  "batch": 105,
  "quantity": 480,
  "date": "2026-07-20T14:30:00+02:00",
  "quality": 1
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `415` |
| [`batch`](batch.md) | integer | Pulse `id` of the batch associated with the output. | `105` |
| `quantity` | number | Quantity of output recorded. | `480` |
| `date` | string | Timestamp when the output was recorded, in ISO 8601 format. | `"2026-07-20T14:30:00+02:00"` |
| `quality` | enum | Quality classification of the produced output: bad (`0`) or good (`1`). Bad-quality output is still recorded as produced and should not be submitted as waste. | `1` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `ProducedService` | `/services/pulse/manufacturing/batches/produced` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Batch](batch.md) 

Create or retrieve the batch before submitting the produced record.
