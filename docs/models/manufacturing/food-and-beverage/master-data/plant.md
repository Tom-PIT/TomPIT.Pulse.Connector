# Plant

Represents a physical or organizational operating location in Pulse.

## The Plant object

```json
{
  "code": "PLANT-LJ",
  "name": "Ljubljana plant"
  "attributes": {
    "country": "SI"
  }
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the plant in external systems and integrations. | `"PLANT-LJ"` |
| `name` | string | Human-readable name of the plant. | `"Ljubljana plant"` |
| `attributes` | object or null | Optional additional source-system attributes associated with the plant. These values are stored with the plant but are not used for analysis. | `{ "country": "SI" }` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `PlantService` | `/services/pulse/food-beverage/plants` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Referenced by

- [Production lines](production-line.md)