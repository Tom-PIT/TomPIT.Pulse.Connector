# Maintenance

Represents preventive or corrective maintenance work performed on a machine.

A maintenance record can be created when work is planned, when work starts, or after the work has finished. Planned and actual timing use the same resource and can be submitted as the information becomes available.

## The Maintenance object

```json
{
  "code": "WO-8830",
  "equipment": "PASTEURIZER-01",
  "kind": "preventive",
  "reason": "MNT-PLATE-PACK-INSPECTION",
  "plannedStart": "2026-08-14T08:00:00+02:00",
  "plannedEnd": "2026-08-14T11:00:00+02:00",
  "at": "2026-08-14T08:12:00+02:00",
  "end": "2026-08-14T10:48:00+02:00"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the maintenance activity in source systems and integrations. | `"WO-8830"` |
| [`equipment`](../master-data/machine.md) | string | Code of the machine, component, or wear part being maintained. | `"PASTEURIZER-01"` |
| `kind` | string | Fundamental nature of the maintenance work: `preventive` or `corrective`. | `"preventive"` |
| [`reason`](../master-data/reason.md) | string or null | Optional reason associated with the maintenance activity. | `"MNT-PLATE-PACK-INSPECTION"` |
| `plannedStart` | string or null | Optional planned start timestamp, in ISO 8601 format with an explicit offset. | `"2026-08-14T08:00:00+02:00"` |
| `plannedEnd` | string or null | Optional planned end timestamp, in ISO 8601 format with an explicit offset. | `"2026-08-14T11:00:00+02:00"` |
| `at` | string or null | Actual timestamp when maintenance work started, in ISO 8601 format with an explicit offset. | `"2026-08-14T08:12:00+02:00"` |
| `end` | string or null | Actual timestamp when maintenance work ended, in ISO 8601 format with an explicit offset. | `"2026-08-14T10:48:00+02:00"` |
| `items` | array or null | Optional planned resource requirements associated with the maintenance work. | — |

</div>

## Maintenance kind

`kind` describes **why the maintenance is being performed**.

| Kind | Meaning |
| --- | --- |
| `preventive` | Work performed to prevent failure, deterioration, or future downtime. |
| `corrective` | Work performed because a defect, failure, deterioration, or other problem already exists. |

This classification is independent of whether the work was planned.

## Planned and unplanned maintenance

Planned and unplanned maintenance are determined by the presence of a planned time window.

A maintenance activity with `plannedStart` and `plannedEnd` is planned.

A maintenance activity without a planned window is unplanned.

This is separate from `kind`.

| | Preventive | Corrective |
| --- | --- | --- |
| **Planned** | Scheduled inspection or service | Known defect scheduled for the next planned stop |
| **Unplanned** | Opportunistic preventive work during another interruption | Breakdown requiring immediate repair |

For example, a corrective job can still be planned:

```json
{
  "code": "WO-8851",
  "equipment": "PUMP-04",
  "kind": "corrective",
  "reason": "MNT-SEAL-LEAK",
  "plannedStart": "2026-08-15T06:00:00+02:00",
  "plannedEnd": "2026-08-15T07:00:00+02:00"
}
```

The defect already exists, so the work is corrective. Because the repair was scheduled before execution, it is also planned.

There is no separate `unplanned` flag. Omitting the planned window declares the work unplanned.

## Maintenance lifecycle

Maintenance information can be submitted as it becomes available.

A work order can first be submitted when it is scheduled:

```json
{
  "code": "WO-8830",
  "equipment": "PASTEURIZER-01",
  "kind": "preventive",
  "reason": "MNT-PLATE-PACK-INSPECTION",
  "plannedStart": "2026-08-14T08:00:00+02:00",
  "plannedEnd": "2026-08-14T11:00:00+02:00"
}
```

When the work starts, submit the same `code` with the actual start:

```json
{
  "code": "WO-8830",
  "at": "2026-08-14T08:12:00+02:00"
}
```

When it finishes:

```json
{
  "code": "WO-8830",
  "end": "2026-08-14T10:48:00+02:00"
}
```

Fields omitted from later requests remain unchanged.

A source system that only exports completed work orders can submit planned and actual timestamps together in one request.

## Planned and actual duration

Keeping the planned window and actual maintenance timing together allows Pulse to compare what was scheduled with what actually happened.

For example:

```text
Planned:  08:00 ───────────────── 11:00
Actual:     08:12 ─────────── 10:48
```

This allows maintenance performance to be analysed in terms such as:

- planned work that was never executed;
- maintenance that started late;
- work that exceeded its planned window;
- planned versus unplanned maintenance time.

## Resource consumption

Actual materials, labor, equipment, energy, and other resources used during maintenance are recorded through [Consumption](../manufacturing/consumption.md).

For example:

```json
{
  "maintenance": "WO-8830",
  "category": "ingredient",
  "item": "MAT-GASKET-P3",
  "quantity": 8,
  "unit": "pcs",
  "unitValue": 4.20,
  "at": "2026-08-14T09:10:00+02:00"
}
```

Labor is recorded in the same way using a registered crew:

```json
{
  "maintenance": "WO-8830",
  "category": "labour",
  "item": "CREW-MAINT-A",
  "quantity": 3,
  "unit": "h",
  "unitValue": 31.00,
  "at": "2026-08-14T11:00:00+02:00"
}
```

This replaces the separate maintenance material, labor, equipment, energy, and expense usage resources from the previous API model.

## Maintenance and line states

A maintenance activity does not automatically mean that the production line stopped.

For example, preventive maintenance on a standby machine may have no effect on production availability.

When maintenance also stops the line, record the corresponding [Line state](../manufacturing/line-state.md) separately.

Use:

| Situation | Line state |
| --- | --- |
| Breakdown that stops the line | `breakdown` |
| Scheduled maintenance that stops the line | `planned-maintenance` |

For example:

```json
{
  "state": "planned-maintenance",
  "reason": "MNT-PLATE-PACK-INSPECTION",
  "from": "2026-08-14T08:12:00+02:00",
  "to": "2026-08-14T10:48:00+02:00"
}
```

The maintenance record answers:

> What maintenance work was performed on the machine?

The line state answers:

> How did that work affect production-line time?

Keeping the two separate prevents maintenance work from being incorrectly counted as production downtime when the line continued operating.

## API resource

| Resource | Base path |
| --- | --- |
| Maintenance | `/services/pulse/food-beverage/maintenance` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Machine](../master-data/machine.md)
- [Reason](../master-data/reason.md), when provided

The referenced machine must be available before submitting the maintenance activity.

When `reason` is provided as a shared reason code, the referenced reason must also be available.