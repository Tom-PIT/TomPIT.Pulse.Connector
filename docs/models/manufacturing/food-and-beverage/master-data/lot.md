# Lot

Represents a traceable quantity of material received or produced in Food & Beverage operations.

A lot identifies the specific material quantity involved in production and provides the traceability context needed to distinguish deliveries and produced quantities of the same material.

## The Lot object

```json
{
  "code": "MILK-2026-0717-A",
  "material": "MILK-RAW",
  "supplier": "SUP-001",
  "receivedAt": "2026-07-17T05:45:00+02:00",
  "expiresAt": "2026-07-24T23:59:59+02:00",
  "quantity": 1000,
  "unit": "kg",
  "types": {
    "lotType": "INCOMING"
  },
  "attributes": {
    "deliveryNote": "DN-2026-1844"
  }
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the lot in source systems and integrations. | `"MILK-2026-0717-A"` |
| `material` | string | Code of the material associated with the lot. | `"MILK-RAW"` |
| `supplier` | string or null | Optional code of the supplier from which an incoming lot was received. | `"SUP-001"` |
| `receivedAt` | string or null | Optional timestamp when the lot was received, in ISO 8601 format with an explicit offset. | `"2026-07-17T05:45:00+02:00"` |
| `expiresAt` | string or null | Optional timestamp when the lot expires, in ISO 8601 format with an explicit offset. | `"2026-07-24T23:59:59+02:00"` |
| `quantity` | number or null | Optional quantity associated with the lot. | `1000` |
| `unit` | string or null | Unit in which the lot quantity is expressed. | `"kg"` |
| `types` | object or null | Optional classifications used to group and analyse the lot. | `{ "lotType": "INCOMING" }` |
| `attributes` | object or null | Optional additional source-system metadata associated with the lot. These values are stored but are not used for analysis. | `{ "deliveryNote": "DN-2026-1844" }` |

</div>

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## Traceability

A lot identifies a specific quantity of material rather than only the general material definition.

For example, two deliveries of the same raw material can have different suppliers, composition, age, or expiry dates. Keeping their lot codes separate allows Pulse to preserve that distinction throughout production.

Quantities consumed during production are recorded separately. Consumption records reference the lot that was actually used.

Produced lots can also be used as inputs to later production steps, preserving traceability across process batches and production runs.

## Lot analysis

Measured properties associated with a lot, such as fat, protein, moisture, or other composition values, can be submitted through the lot analysis resource.

`POST /services/pulse/food-beverage/lots/{code}/analysis`

Lot analysis results are measurements, not attributes or types.

## API resource

| Resource | Base path |
| --- | --- |
| Lot | `/services/pulse/food-beverage/lots` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Material](material.md)
- [Supplier](supplier.md)

The material referenced by `material` must be available before submitting the lot.

When `supplier` is provided, the referenced supplier must also be available.