# Batch

Represents a specific operational unit of work tracked in Pulse.

Although Batch belongs to the Manufacturing API family, it is not limited to production. A batch can represent a production series, supply activity, logistics operation, service process, or another grouped execution that Pulse evaluates as one unit.

## The Batch object

```json
{
  "id": 105,
  "product": 42,
  "productionLine": 24,
  "code": "BATCH-2026-0015",
  "price": 125.00,
  "customer": 36
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `105` |
| [`product`](../master-data/product.md) | integer | Pulse `id` of the product or other output associated with the batch. | `42` |
| [`productionLine`](../master-data/production-line.md) | integer | Pulse `id` of the production line or execution unit associated with the batch. | `24` |
| `code` | string | Business code used to identify the batch in source systems, traceability records, and integrations. | `"BATCH-2026-0015"` |
| `price` | number or null | Price or revenue value associated with the batch or its output in the context of this execution. | `125.00` |
| [`customer`](../master-data/customer.md) | integer or null | Pulse `id` of the customer associated with the batch, when applicable. | `36` |

</div>

## Price

`price` provides the revenue or valuation context for the batch. It can represent a sales price, agreed batch value, internal valuation, planned revenue basis, or another value supplied by the source system.

## API service

| Service | Base path |
| --- | --- |
| `BatchService` | `/services/pulse/manufacturing/batches` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Product](../master-data/product.md) 
- [Production line](../master-data/production-line.md) 

May also reference: 

- [Customer](../master-data/customer.md). 

Create or retrieve these records before submitting the batch.

## Referenced by

- [Batch plans](batch-plan.md)
- [Batch usage records](batch-usage.md)
- [Batch shift records](batch-shift.md)
- [Produced records](produced.md)
- [Stages](stage.md)