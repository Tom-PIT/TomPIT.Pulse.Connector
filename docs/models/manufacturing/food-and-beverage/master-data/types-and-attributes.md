# Types and attributes

Food & Beverage master data can include extensible classification and metadata.

Use **types** to define controlled classifications that Pulse can use in analysis.

Use **attributes** for additional source-system metadata that should be stored but not analysed.

## Types

Types define controlled classifications such as allergen, storage class, origin, region, or pack format.

A type is declared before its values are referenced by master data.

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

Master-data resources reference declared type values through fields defined by that resource.

For example, a material can reference a storage classification:

```json
{
  "code": "MILK-RAW",
  "name": "Raw milk",
  "storage": "CHILLED"
}
```

A resource can reference several independent classifications when its API defines the corresponding fields:

```json
{
  "code": "MILK-RAW",
  "name": "Raw milk",
  "allergen": "ALG-MILK",
  "storage": "CHILLED",
  "origin": "SI"
}
```

Pulse can use these classifications when comparing and analysing records.

> [!IMPORTANT]
> Master-data records do not use a generic `types` object. Each resource defines the classification fields it supports.

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

Ask whether the property could help explain why one production run performs differently from another.

If yes, use a classification field supported by that resource and reference a value declared through the Types resource.

If no, use an attribute.

For example:

| Property | Use |
| --- | --- |
| Allergen group | Classification field |
| Storage class | Classification field |
| Origin region | Classification field |
| Pack format | Classification field |
| ERP reference | `attributes` |
| Drawing revision | `attributes` |
| Serial number | `attributes` |
| Internal notes | `attributes` |

Not every classification applies to every resource. Check the resource documentation for the classification fields it supports.

Measured values that vary over time, such as temperature, protein, or moisture, are neither classifications nor attributes. They should be submitted as measurements.

## API resource

Types are declared through:

`/services/pulse/food-beverage/types`

See the individual master-data resource documentation for the classification fields supported by each resource.