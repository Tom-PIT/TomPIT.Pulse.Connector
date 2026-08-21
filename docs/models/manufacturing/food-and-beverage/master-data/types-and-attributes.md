# Types and attributes

Food & Beverage master data can include two kinds of extensible properties: `types` and `attributes`.

Use `types` for classifications that Pulse should be able to analyse.

Use `attributes` for additional source-system metadata that should be stored but not analysed.

## Types

Types define controlled classifications such as allergen, storage class, origin, region, or pack format.

A type is declared before it is assigned to master data.

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

A material can then reference the declared type:

```json
{
  "code": "MILK-RAW",
  "name": "Raw milk",
  "types": {
    "storage": "CHILLED"
  }
}
```

A record can use several independent types:

```json
"types": {
  "allergen": "ALG-MILK",
  "storage": "CHILLED",
  "origin": "SI"
}
```

Pulse can use these classifications when comparing and analysing records.

## Attributes

Attributes are additional source-system properties that do not need to participate in analysis.

They are supplied as a free-form JSON object:

```json
{
  "code": "PLANT-LJ",
  "name": "Ljubljana plant",
  "attributes": {
    "erpCode": "1000",
    "country": "SI"
  }
}
```

Attribute names and values do not need to be declared in advance.

Pulse stores these values with the record but does not use them for analysis.

## Choosing between types and attributes

Ask whether the property could help explain why one production run performs differently from another.

If yes, use a type when the property is a stable classification.

If no, use an attribute.

For example:

| Property | Use |
| --- | --- |
| Allergen group | `types` |
| Storage class | `types` |
| Origin region | `types` |
| Pack format | `types` |
| ERP reference | `attributes` |
| Drawing revision | `attributes` |
| Serial number | `attributes` |
| Internal notes | `attributes` |

Measured values that vary over time, such as temperature, protein, or moisture, are neither types nor attributes. They should be submitted as measurements.

## API resource

Types are declared through:

`/services/pulse/food-beverage/types`

See the [API reference](../api/index.md) for supported operations and complete request schemas.
