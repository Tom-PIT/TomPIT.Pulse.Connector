# Event

Represents a discrete occurrence that happened during Food & Beverage operations.

Events are used for occurrences such as stoppages, deviations, waste, rework, and rejects when the occurrence itself is analytically meaningful.

Unlike lifecycle work such as a run or clean, an event does not represent work that consumes resources of its own.

## The Event object

```json
{
  "type": "stoppage",
  "line": "L03",
  "run": "L03-260810-002",
  "reason": "DTCU010",
  "severity": 2,
  "at": "2026-08-10T23:17:00+02:00",
  "end": "2026-08-10T23:24:00+02:00"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `type` | string | Declared event type describing what happened. | `"stoppage"` |
| [`run`](run.md) | string or null | Optional code of the production run associated with the event. | `"L03-260810-002"` |
| [`batch`](batch.md) | string or null | Optional code of the process batch associated with the event. | `"BULK-260810-07"` |
| [`line`](../master-data/production-line.md) | string or null | Optional code of the production line on which the event occurred. | `"L03"` |
| [`lot`](../master-data/lot.md) | string or null | Optional code of the lot associated with the event. | `"MILK-2026-0717-A"` |
| [`reason`](../master-data/reason.md) | string or null | Optional reason code explaining why the event occurred. | `"DTCU010"` |
| `severity` | number or null | Optional severity assigned to the occurrence. | `2` |
| `at` | string | Timestamp when the event occurred or began, in ISO 8601 format with an explicit offset. | `"2026-08-10T23:17:00+02:00"` |
| `end` | string or null | Optional timestamp when an interval event ended. | `"2026-08-10T23:24:00+02:00"` |

</div>

The context fields used depend on the type of event.

For example, a stoppage can identify both the production run that was active and the line on which the interruption occurred.

A waste event during bulk processing can reference a batch instead of a run.

## Event types

The Food & Beverage profile includes event types such as:

| Type | Typical meaning |
| --- | --- |
| `waste` | Trim, flush, spillage, expired work in progress, or another lost quantity. |
| `rework` | Product recovered into later production. |
| `downgrade` | Product retained at a lower grade or value. |
| `reject-metal` | Product rejected by metal detection. |
| `reject-xray` | Product rejected by X-ray or foreign-body detection. |
| `reject-weight` | Product rejected because its weight is outside the permitted range. |
| `reject-seal` | Product rejected because of seal or closure integrity. |
| `reject-vision` | Product rejected by a vision inspection such as label, code, or fill-level inspection. |
| `stoppage` | A countable production interruption. |
| `spec-deviation` | A measured value outside a declared specification. |
| `ccp-deviation` | A deviation at a critical control point. |
| `complaint` | A customer-side quality occurrence. |

The event types available to an integration are defined by the active Food & Beverage profile. :contentReference[oaicite:1]{index=1}

## Events and line states

An [Event](event.md) and a [Line state](line-state.md) can describe different aspects of the same occurrence.

For example, consider a line that stops because of a mechanical fault.

The event records that a stoppage happened:

```json
{
  "type": "stoppage",
  "line": "L03",
  "run": "L03-260810-002",
  "reason": "MECH-BEARING",
  "severity": 2,
  "at": "2026-08-10T23:17:00+02:00",
  "end": "2026-08-10T23:24:00+02:00"
}
```

The corresponding line state accounts for the line time:

```json
{
  "state": "breakdown",
  "reason": "MECH-BEARING",
  "from": "2026-08-10T23:17:00+02:00",
  "to": "2026-08-10T23:24:00+02:00"
}
```

The distinction is:

```text
Event      → something happened
Line state → how line time was spent
```

The event is countable and can carry severity and production context.

The line state contributes to continuous time accounting.

Keeping them separate allows Pulse to answer both questions:

- How often did breakdowns occur?
- How much production time was lost to breakdowns?

Collapsing the two would lose either the event count or the line-time accounting. :contentReference[oaicite:2]{index=2}

## Run and batch context

Events should be associated with the production context in which they actually occurred.

For example, waste during filling belongs to the run:

```json
{
  "type": "waste",
  "run": "L03-260810-002",
  "at": "2026-08-10T23:40:00+02:00"
}
```

Waste during a cook or other bulk process can instead belong to the batch:

```json
{
  "type": "waste",
  "batch": "BULK-260810-07",
  "at": "2026-08-10T19:20:00+02:00"
}
```

This distinction prevents a process loss from being incorrectly attributed to a later packing run, or a packing loss from being attributed back to the process batch. :contentReference[oaicite:3]{index=3}

## Interval events

Some events happen at a single instant.

Others, such as a stoppage, may have a duration.

For an interval event, use `at` for the beginning and `end` for the end:

```json
{
  "type": "stoppage",
  "line": "L03",
  "at": "2026-08-10T23:17:00+02:00",
  "end": "2026-08-10T23:24:00+02:00"
}
```

For an instantaneous occurrence, omit `end`.

## API resource

| Resource | Base path |
| --- | --- |
| Event | `/services/pulse/food-beverage/events` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

Depending on the event context, the event may reference:

- [Run](run.md)
- [Batch](batch.md)
- [Production line](../master-data/production-line.md)
- [Lot](../master-data/lot.md)
- [Reason](../master-data/reason.md), when provided

Referenced records must be available before submitting the event.