# Machine

Represents a machine, equipment asset, component, or wear part associated with a production line.

Machines can be organised hierarchically. For example, a filler head or nozzle can be registered as a child of a larger machine.

## The Machine object

```json
{
  "code": "PASTEURIZER-01",
  "name": "Pasteurizer 01",
  "line": "YOGURT-LINE-01",
  "parent": null,
  "types": {
    "machineType": "PASTEURIZER"
  },
  "attributes": {
    "manufacturer": "ExampleCo",
    "installed": 2019
  }
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the machine in source systems and integrations. | `"PASTEURIZER-01"` |
| `name` | string | Human-readable name of the machine. | `"Pasteurizer 01"` |
| `line` | string | Code of the production line to which the machine belongs. | `"YOGURT-LINE-01"` |
| `parent` | string or null | Optional code of the parent machine. Use this to represent machine components and wear parts hierarchically. | `null` |
| `types` | object or null | Optional classifications used to group and analyse the machine. | `{ "machineType": "PASTEURIZER" }` |
| `attributes` | object or null | Optional additional source-system metadata associated with the machine. These values are stored but are not used for analysis. | `{ "manufacturer": "ExampleCo", "installed": 2019 }` |

</div>

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## Machine hierarchy

Machines can contain other machines or components.

For example:

```text
Filler
└── Filler head 6
    └── Nozzle
```

A child machine references its parent using the `parent` field:

```json
{
  "code": "FILLER-01-NOZZLE-06",
  "name": "Filler nozzle 6",
  "line": "YOGURT-LINE-01",
  "parent": "FILLER-01"
}
```

## API resource

| Resource | Base path |
| --- | --- |
| Machine | `/services/pulse/food-beverage/machines` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Production line](production-line.md)

The production line referenced by `line` must be available before submitting the machine.

When `parent` is provided, the referenced parent machine must also be available.