# Delay

Defines a type of delay in Pulse.

## The Delay object

```json
{
  "id": 19,
  "code": "MATERIAL_SHORTAGE",
  "name": "Material shortage"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `19` |
| `code` | string | Business code used to identify the delay in external systems and integrations. | `"MATERIAL_SHORTAGE"` |
| `name` | string | Human-readable name of the delay. | `"Material shortage"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `DelayService` | `/services/pulse/types/delays` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Referenced by

- [Stage delay records](../manufacturing/stage-delay.md)
