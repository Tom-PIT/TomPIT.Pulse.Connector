# Lot

Represents a traceable lot, batch, or group of material, product, semi-finished product, spare part, or another operational resource.

A lot identifies the specific source or group of units that was actually used, received, produced, or otherwise tracked. It adds traceability beyond the general material or product code.

Two lots of the same material or product may differ in supplier, quality, age, expiry date, stability, or behavior during execution. By preserving the lot reference, Pulse can distinguish between otherwise similar operational records and identify patterns connected to a specific delivery or traceable group.

## The Lot object

```json
{
  "id": 92,
  "code": "LOT-2026-0719-A",
  "supplier": 18,
  "customer": null,
  "created": "2026-07-19T07:30:00+02:00",
  "expire": "2027-01-19T23:59:59+01:00"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `92` |
| `code` | string | Lot, batch, or traceability code used by the source system or organization. | `"LOT-2026-0719-A"` |
| [`supplier`](../master-data/supplier.md) | integer or null | Optional Pulse `id` of the supplier associated with the lot. | `18` |
| [`customer`](../master-data/customer.md) | integer or null | Optional Pulse `id` of the customer associated with the lot. | `null` |
| `created` | string or null | Optional timestamp when the lot was created, received, or recorded, in ISO 8601 format. | `"2026-07-19T07:30:00+02:00"` |
| `expire` | string or null | Optional timestamp after which the lot is no longer valid, usable, or quality-acceptable, in ISO 8601 format. | `"2027-01-19T23:59:59+01:00"` |

</div>

## Traceability context

A lot does not define how much was consumed. It identifies which traceable group was involved.

The quantity remains on the related usage record, while the lot adds source and traceability context. For example, a material usage record identifies the quantity consumed, and its optional `lot` field identifies the specific lot from which that material came.

This distinction allows Pulse to compare results not only by material or product, but also by the exact lot used.

## Supplier and customer relationships

A lot may reference a [supplier](../master-data/supplier.md) when it originates from a specific delivery or external source.

It may also reference a [customer](../master-data/customer.md) when the lot is associated with customer-specific production, an order, contractual requirements, complaints, or outbound traceability.

These relationships are optional because not every source system provides them for every lot.

## Time context

`created` and `expire` preserve the time context of the lot when that information is available.

`created` may represent when the lot was created, received, manufactured, or first recorded.

`expire` may represent the end of its shelf life, validity, usability, or quality acceptance period.

This information can help Pulse identify whether operational differences are associated with lot age, storage duration, or proximity to expiry.

## API service

| Service | Base path |
| --- | --- |
| `LotService` | `/services/pulse/manufacturing/lots` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

May reference:

- [Supplier](../master-data/supplier.md)
- [Customer](../master-data/customer.md)

Create or retrieve the applicable records before submitting the lot.

## Referenced by

- [Material usage records](../manufacturing/material-usage.md)
- [Energy source usage records](../manufacturing/energy-source-usage.md)