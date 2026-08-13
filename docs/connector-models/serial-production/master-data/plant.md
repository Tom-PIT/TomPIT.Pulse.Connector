# Plant

Represents a physical or organizational operating location in Pulse.

## The Plant object

```json
{
  "id": 12,
  "code": "PLANT-LJ",
  "name": "Ljubljana plant"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `12` |
| `code` | string | Business code used to identify the plant in external systems and integrations. | `"PLANT-LJ"` |
| `name` | string | Human-readable name of the plant. | `"Ljubljana plant"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `PlantService` | `/services/pulse/types/plants` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Referenced by

- [Production lines](production-line.md)