# Master data

Master data defines the relatively stable business entities and code lists referenced by operational data submitted to Pulse.

It includes plants, production lines, products, materials, equipment, shifts, units of measure, suppliers, and classification records such as downtime types and waste types.

Synchronize the required code lists before submitting operational records that reference them.

## Available master data

### Organization and production structure

| Resource | Purpose |
| --- | --- |
| **Plant** | Represents a physical or organizational operating location. |
| **Production line** | Represents a production line associated with a plant. |
| **Equipment** | Represents a machine, asset, or other equipment used during an activity. |
| **Shift** | Represents a defined work shift. |

### Products and resources

| Resource | Purpose |
| --- | --- |
| [**Product**](product.md) | Represents an output or item produced during an activity. |
| [**Material**](material.md) | Represents material consumed or referenced during an activity. |
| [**Measure unit**](measure-unit.md) | Defines the unit used for quantities and measurements. |
| **Labour** | Represents a labour category or resource used during an activity. |
| **Energy source** | Represents a type of energy consumed during an activity. |
| **Expense** | Represents an additional type of cost. |
| **Supplier** | Represents the supplier associated with material or energy usage. |

### Classification data

| Resource | Purpose |
| --- | --- |
| **Downtime category** | Groups related downtime types. |
| **Downtime type** | Defines a planned or unplanned type of downtime. |
| **Downtime cause** | Defines the cause associated with downtime. |
| **Waste type** | Defines a classification for waste. |
| **Delay** | Defines a type of delay. |
| **Ambient type** | Defines a measurement type, its unit, and expected value range. |

See the [API reference](../../api/index.md) for the available services and endpoint paths.

## Identifiers

Master-data and code-list records generally use the following identifiers:

| Field | Assigned by | Purpose |
| --- | --- | --- |
| `Code` | Source system | Identifies the record in external systems and is used to retrieve it from Pulse. |
| `Id` | Pulse | Identifies the record in Pulse and is used when other records reference it. |

Fields such as `Plant`, `MeasureUnit`, and `Category` contain the Pulse `Id` of a related record. They define relationships between records rather than additional identifiers for the current record.

For example:

| Source-system record | Pulse record |
| --- | --- |
| `Code: PRODUCT-001` | `Id: 21` |

When an insert operation succeeds, Pulse commonly returns the numeric `Id` assigned to the new record.

When another request requires the Pulse `Id`, retrieve the corresponding record by its `Code` and use the returned `Id`.

## Dependencies

Some master-data records refer to other master-data records.

```mermaid
graph LR
  A[Product] --> B[Measure unit]
  C[Material] --> B[Measure unit]
  D[Energy source] --> B[Measure unit]
  I[Ambient type] --> B[Measure unit]
  F[Production line] --> E[Plant]
  H[Downtime type] --> G[Downtime category]
  
```

For example:

- A production line references a plant.
- A product references a measure unit.
- A material references a measure unit.
- An energy source references a measure unit.
- A downtime type references a downtime category.
- An ambient type references a measure unit.

Create or retrieve the referenced record before submitting the dependent record.

## Example: product mapping

Suppose your source system contains:

| Field | Value |
| --- | --- |
| Product code | `PRODUCT-001` |
| Name | `Product 001` |
| Unit | `piece` |

Before submitting the product, obtain the Pulse `Id` of the corresponding `piece` unit.

The request body then uses that Pulse identifier:

```json
{
  "MeasureUnit": 1,
  "Code": "PRODUCT-001",
  "Name": "Product 001"
}
```

A successful insert returns the Pulse identifier for the newly created record:

```json
21
```

When another request needs to reference this product, retrieve the product by its `Code`.

Pulse returns the corresponding record, including its `Id`:

```json
{
  "Id": 21,
  "Code": "PRODUCT-001",
  "Name": "Product 001",
  "MeasureUnit": 1,
  "Price": 4.00
}
```

Use the returned `Id` in the related request.


## Synchronization approach

A master-data synchronization typically performs these steps:

1. Read the relevant records from the source system.
2. Transform each record into the Pulse request format.
3. Submit the corresponding record to Pulse.
4. Verify that the record was submitted successfully.
5. Retrieve Pulse identifiers by `Code` when related records require them.
6. Log failures for retry or correction.

Use [Scalar](https://scalar.com/) to inspect endpoint schemas and test individual requests. Implement recurring or bulk synchronization in your integration application or service.