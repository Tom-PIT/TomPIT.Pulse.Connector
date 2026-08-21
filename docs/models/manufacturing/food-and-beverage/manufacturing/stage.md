# Stage

Represents an execution step within a production [run](run.md).

A stage identifies when a specific part of the production process was active. It can optionally name the machine responsible for that step.

Examples include filling, sealing, labelling, inspection, or case packing.

## The Stage object

```json
{
  "run": "L03-260810-002",
  "code": "fill",
  "machine": "L03-filler",
  "at": "2026-08-10T22:10:00+02:00"
}
```

When the stage finishes, submit the same stage with its end time:

```json
{
  "run": "L03-260810-002",
  "code": "fill",
  "end": "2026-08-11T04:15:00+02:00"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| [`run`](run.md) | string | Code of the production run to which the stage belongs. | `"L03-260810-002"` |
| `code` | string | Business code used to identify the stage within the run. | `"fill"` |
| [`machine`](../master-data/machine.md) | string or null | Optional code of the machine associated with the stage. | `"L03-filler"` |
| `at` | string | Timestamp when the stage started, in ISO 8601 format with an explicit offset. | `"2026-08-10T22:10:00+02:00"` |
| `end` | string or null | Timestamp when the stage ended, in ISO 8601 format with an explicit offset. | `"2026-08-11T04:15:00+02:00"` |

</div>

## Stage lifecycle

A stage can be submitted when it starts:

```json
{
  "run": "L03-260810-002",
  "code": "fill",
  "machine": "L03-filler",
  "at": "2026-08-10T22:10:00+02:00"
}
```

When the stage finishes, submit the fields that became known:

```json
{
  "run": "L03-260810-002",
  "code": "fill",
  "end": "2026-08-11T04:15:00+02:00"
}
```

Fields omitted from the second request remain unchanged.

When the parent run is closed, any stage that is still open is closed at the same time.

## Stages and machines

A machine should be associated with the stage during which it actually operates rather than with the entire run.

For example:

```text
Run
├── Fill        → Filler
├── Seal        → Sealer
└── Case pack   → Case packer
```

This allows Pulse to distinguish the operating time of individual machines.

A run may last eight hours while a particular machine operates for only six of those hours. Associating the machine with its stage preserves that distinction.

## API resource

| Resource | Base path |
| --- | --- |
| Stage | `/services/pulse/food-beverage/stages` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Run](run.md)
- [Machine](../master-data/machine.md), when provided

The run must be available before submitting the stage.

When `machine` is provided, the referenced machine must also be available.