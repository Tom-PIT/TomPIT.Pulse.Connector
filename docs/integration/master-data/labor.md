# Labor

Represents a labor category or resource used during an activity in Pulse.

## The Labor object

```json
{
  "id": 18,
  "code": "TECHNICIAN",
  "name": "Maintenance technician",
  "price": 32.50,
  "description": "Skilled technician responsible for maintenance activities"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `18` |
| `code` | string | Business code used to identify the labor resource in external systems and integrations. | `"TECHNICIAN"` |
| `name` | string | Human-readable name of the labor resource. | `"Maintenance technician"` |
| `price` | number or null | Optional default hourly price. Pulse may use this value when a related operational record does not provide its own price. | `32.50` |
| `description` | string or null | Optional description of the labor resource and its role. | `"Skilled technician responsible for maintenance activities"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `LaborService` | `/services/pulse/types/labor` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Referenced by

- [Labor plans](../manufacturing/labor-plan.md)
- [Labor usage records](../manufacturing/labor-usage.md)