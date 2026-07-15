# Equipment

Represents a machine, asset, or other equipment resource tracked in Pulse.

## The Equipment object

```json
{
  "id": 31,
  "code": "CNC-01",
  "name": "CNC milling machine",
  "price": 85.00
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `31` |
| `code` | string | Business code used to identify the equipment in external systems and integrations. | `"CNC-01"` |
| `name` | string | Human-readable name of the equipment. | `"CNC milling machine"` |
| `price` | number or null | Default hourly price used as the baseline for labour-related calculations. | `85.00` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `EquipmentService` | `/services/pulse/types/equipment` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.