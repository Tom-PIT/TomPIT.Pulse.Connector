# Energy source

Represents a type of energy consumed during an activity in Pulse.

## The Energy source object

```json
{
  "id": 14,
  "measureUnit": 8,
  "code": "ELECTRICITY",
  "name": "Electricity",
  "price": 0.18
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
| `price` | number or null | Default price per unit used as the baseline for energy-related calculations. | `0.18` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `EnergySourceService` | `/services/pulse/types/energy-sources` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

Energy sources reference a [measure unit](measure-unit.md). Create or retrieve the measure unit before submitting the energy source.

## Used by

Energy sources are referenced by:

- Energy source plans
- Energy source usage records
- Waste energy source usage records
