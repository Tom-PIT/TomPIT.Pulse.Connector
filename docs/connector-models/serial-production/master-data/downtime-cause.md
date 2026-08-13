# Downtime cause

Defines a cause associated with downtime in Pulse.

## The Downtime cause object

```json
{
  "id": 11,
  "code": "MECHANICAL_FAILURE",
  "name": "Mechanical failure"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `11` |
| `code` | string | Business code used to identify the downtime cause in external systems and integrations. | `"MECHANICAL_FAILURE"` |
| `name` | string | Human-readable name of the downtime cause. | `"Mechanical failure"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `DowntimeCauseService` | `/services/pulse/types/downtime-causes` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Referenced by

- [Downtime records](../manufacturing/downtime.md)