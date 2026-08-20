# Equipment

Represents a machine, asset, or other equipment resource tracked in Pulse.

## The Equipment object

```json
{
  "id": 31,
  "code": "PASTEURIZER-01",
  "name": "Pasteurizer 01",
  "price": 85.00,
  "description": "Pasteurizer used for thermal processing of the yogurt mixture"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `31` |
| `code` | string | Business code used to identify the equipment in external systems and integrations. | `"PASTEURIZER-01"` |
| `name` | string | Human-readable name of the equipment. | `"Pasteurizer 01"` |
| `price` | number or null | Optional default hourly price. Pulse may use this value when a related operational record does not provide its own price. | `85.00` |
| `description` | string or null | Optional description of the equipment and its role in the process. | `"Pasteurizer used for thermal processing of the yogurt mixture"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `EquipmentService` | `/services/pulse/types/equipment` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Referenced by

- [Equipment plans](../manufacturing/equipment-plan.md)
- [Equipment usage records](../manufacturing/equipment-usage.md)
- [Downtime records](../manufacturing/downtime.md)
- [Maintenance records](../maintenance/maintenance.md)
- [Maintenance equipment plans](../maintenance/maintenance-equipment-plan.md)
- [Maintenance equipment usage records](../maintenance/maintenance-equipment-usage.md)