# Production line

Represents a production line associated with a plant in Pulse.

## The Production line object

```json
{
  "id": 24,
  "plant": 12,
  "code": "LINE-A1",
  "name": "Assembly line 1",
  "status": 1
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `24` |
| [`plant`](plant.md) | integer | Pulse `id` of the plant to which the production line belongs. | `12` |
| `code` | string | Business code used to identify the production line in external systems and integrations. | `"LINE-A1"` |
| `name` | string | Human-readable name of the production line. | `"Assembly line 1"` |
| `status` | enum | Current status of the production line. | `1` |

</div>

> [!NOTE]
> The supported `status` values are not yet documented.

## API service

| Service | Base path |
| --- | --- |
| `ProductionLineService` | `/services/pulse/types/production-lines` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Depends on

Production lines reference a [plant](plant.md). Create or retrieve the plant before submitting the production line.