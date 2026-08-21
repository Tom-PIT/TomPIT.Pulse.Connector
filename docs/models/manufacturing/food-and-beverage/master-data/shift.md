# Shift

Represents a defined work period used to organize operational activity in Pulse.

## The Shift object

```json
{
  "code": "MORNING",
  "name": "Morning shift",
  "types": {
    "shiftType": "DAY"
  },
  "attributes": {
    "startsAt": "06:00",
    "endsAt": "14:00"
  }
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the shift in source systems and integrations. | `"MORNING"` |
| `name` | string | Human-readable name of the shift. | `"Morning shift"` |
| `types` | object or null | Optional classifications used to group and analyse the shift. | `{ "shiftType": "DAY" }` |
| `attributes` | object or null | Optional additional source-system metadata associated with the shift. These values are stored but are not used for analysis. | `{ "startsAt": "06:00", "endsAt": "14:00" }` |

</div>

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## API resource

| Resource | Base path |
| --- | --- |
| Shift | `/services/pulse/food-beverage/shifts` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.