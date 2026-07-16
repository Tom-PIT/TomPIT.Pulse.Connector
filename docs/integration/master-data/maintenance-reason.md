# Maintenance reason

Represents the reason for a maintenance activity, intervention, or event in Pulse.

Maintenance reasons help distinguish why maintenance was performed, for example because of a failure, preventive work, inspection, cleaning, adjustment, calibration, part replacement, or a safety requirement.

## The Maintenance reason object

```json
{
  "id": 13,
  "code": "PREVENTIVE",
  "name": "Preventive maintenance"
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier assigned by Pulse. | `13` |
| `code` | string | Business code used to identify the maintenance reason in external systems and integrations. | `"PREVENTIVE"` |
| `name` | string | Human-readable name of the maintenance reason. | `"Preventive maintenance"` |

</div>

## API service

| Service | Base path |
| --- | --- |
| `MaintenanceReasonService` | `/services/pulse/types/maintenance-reasons` |

See the [API reference](../../api/index.md) for supported operations and complete request schemas.

## Referenced by

- [Maintenance records](../maintenance/maintenance.md)