# Energy source

Represents a type of energy consumed during an activity in Pulse.

## The Energy source object

```json
{
  "id": 14,
  "measureUnit": 8,
  "code": "ELECTRICITY",
  "name": "Electricity",
  "price": 0.18,
  "description": "Electricity used during production activities"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `14` |
| [`measureUnit`](measure-unit.md) | integer | Pulse `id` of the measure unit used for the energy source. | `8` |
| `code` | string | Business code used to identify the energy source in external systems and integrations. | `"ELECTRICITY"` |
| `name` | string | Human-readable name of the energy source. | `"Electricity"` |
| `price` | number or null | Optional default price per measure unit. Pulse may use this value when a related record does not provide its own price. | `0.18` |
| `description` | string or null | Optional description of the energy source and its role in the process. | `"Electricity used during production activities"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `EnergySourceService` | `/services/pulse/types/energy-sources` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

An energy source references a [measure unit](measure-unit.md). Create or retrieve the measure unit before submitting the energy source.

## Referenced by

- [Energy source plans](../manufacturing/energy-source-plan.md)
- [Energy source usage records](../manufacturing/energy-source-usage.md)
- [Waste energy source usage records](../manufacturing/waste-energy-source-usage.md)
