# Crew

Represents a team or operator group used to attribute work and labor consumption in Food & Beverage operations.

Pulse records labor at crew level rather than at individual-person level.

## The Crew object

```json
{
  "code": "CREW-C",
  "name": "Night crew C",
  "types": {
    "crewType": "NIGHT"
  },
  "attributes": {
    "erpCode": "TEAM-03"
  }
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the crew in source systems and integrations. | `"CREW-C"` |
| `name` | string | Human-readable name of the crew. | `"Night crew C"` |
| `types` | object or null | Optional classifications used to group and analyse the crew. | `{ "crewType": "NIGHT" }` |
| `attributes` | object or null | Optional additional source-system metadata associated with the crew. These values are stored but are not used for analysis. | `{ "erpCode": "TEAM-03" }` |

</div>

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## API resource

| Resource | Base path |
| --- | --- |
| Crew | `/services/pulse/food-beverage/crews` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.
