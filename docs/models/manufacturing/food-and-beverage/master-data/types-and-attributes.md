# Types and attributes

Food & Beverage master data can include controlled classifications and additional source-system metadata.

Use **types** to define controlled classifications that Pulse can use in analysis.

Use **attributes** for additional source-system metadata that should be stored but not analysed.

## Types

Types define controlled classifications that can be referenced by supported master-data fields.

A type is declared before its values are referenced.

For example:

```json
{
  "code": "storage",
  "name": "Storage class",
  "appliesTo": "material",
  "values": [
    {
      "code": "CHILLED",
      "name": "Chilled"
    },
    {
      "code": "AMBIENT",
      "name": "Ambient"
    }
  ]
}
```

Master-data records do not use a generic `types` object. Classification values are referenced through explicit fields supported by the resource.

For example:

```json
{
  "code": "MILK-RAW",
  "name": "Raw milk",
  "allergen": "ALG-MILK",
  "storage": "CHILLED",
  "origin": "SI"
}
```

## Supported classification fields

The current Food & Beverage master-data resources support these classification fields:

| Resource | Field | Purpose |
| --- | --- | --- |
| [Material](material.md) | `allergen` | Allergen classification of the material. |
| [Material](material.md) | `storage` | Storage classification of the material. |
| [Material](material.md) | `origin` | Country or origin classification of the material. |
| [Product](product.md) | `packFormat` | Packaging format classification of the product. |

The value supplied in each field is the code of a value declared through the Types resource.

For example:

```json
{
  "code": "PRD001",
  "name": "Yogurt Strawberry 125g",
  "packFormat": "CUP"
}
```

and:

```json
{
  "code": "MAT0001",
  "name": "Raw milk 3.8% fat",
  "allergen": "ALG-MILK",
  "storage": "CHILLED",
  "origin": "SI"
}
```

Pulse can use these classifications when comparing and analysing records.

Controlled classifications are declared through the [Types](../definitions-and-rules/type.md) resource. A type defines the classification itself and the allowed value codes that supported master-data fields can reference.

For example, the `allergen` type can define values such as `ALG-MILK`, which can then be referenced by the `allergen` field on a Material.

## Attributes

Attributes are additional source-system properties that do not need to participate in analysis.

They are supplied as a free-form JSON object:

```json
{
  "code": "PLT001",
  "name": "Munich",
  "attributes": {
    "erpCode": "1000",
    "country": "DE"
  }
}
```

Attribute names and values do not need to be declared in advance.

Pulse stores these values with the record but does not use them for analysis.

## Choosing between types and attributes

Use a classification field when the property should participate in analysis and the resource exposes a field for it.

Use an attribute when the property is additional source-system metadata that should be stored but not analysed.

For example:

| Property | Use |
| --- | --- |
| Material allergen | `allergen` classification field |
| Material storage class | `storage` classification field |
| Material origin | `origin` classification field |
| Product pack format | `packFormat` classification field |
| ERP reference | `attributes` |
| Drawing revision | `attributes` |
| Serial number | `attributes` |
| Internal notes | `attributes` |

Measured values that vary over time, such as temperature, protein, or moisture, are neither classifications nor attributes. They should be submitted through the appropriate measurement APIs.

## API resource

Types are declared through:

`/services/pulse/food-beverage/types`