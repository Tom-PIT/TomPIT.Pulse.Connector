# Labour

Represents a labour category or resource used during an activity in Pulse.

## The Labour object

```json
{
  "id": 18,
  "code": "TECHNICIAN",
  "name": "Maintenance technician",
  "price": 32.50
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `18` |
| `code` | string | Business code used to identify the labour resource in external systems and integrations. | `"TECHNICIAN"` |
| `name` | string | Human-readable name of the labour resource. | `"Maintenance technician"` |
| `price` | number or null | Default hourly price used as the baseline for labour-related calculations. | `32.50` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `LaborService` | `/services/pulse/types/labor` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Used by

Labour records are referenced by:

- [Labour plans](labour-plan.md)
- [Labour usage records](labour-usage.md)