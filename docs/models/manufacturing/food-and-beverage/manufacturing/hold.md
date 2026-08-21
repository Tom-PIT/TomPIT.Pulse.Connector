# Hold

Represents a quality hold placed on a specific lot.

A hold records the period during which material or finished product is withheld from normal use while a quality issue is investigated or resolved.

When the hold is resolved, record what happened to the held quantity using a disposition such as release, rework, downgrade, concession, scrap, or destroy.

## The Hold object

```json
{
  "code": "HOLD-2211",
  "lot": "FG-260810-113",
  "reason": "Metal detector challenge failed",
  "quantity": 4200,
  "unit": "pcs",
  "start": "2026-08-11T07:15:00+02:00"
}
```

When the hold is resolved, submit the same `code` with the disposition and end time:

```json
{
  "code": "HOLD-2211",
  "disposition": "downgrade",
  "quantity": 4200,
  "recoveredUnitValue": 0.41,
  "end": "2026-08-12T11:00:00+02:00"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the hold in source systems and integrations. | `"HOLD-2211"` |
| [`lot`](../master-data/lot.md) | string | Code of the lot placed on hold. | `"FG-260810-113"` |
| [`reason`](../master-data/reason.md) | string | Reason for placing the lot on hold. | `"Metal detector challenge failed"` |
| `quantity` | number | Quantity affected by the hold or its disposition. | `4200` |
| `unit` | string | Unit in which the held quantity is expressed. | `"pcs"` |
| `start` | string | Timestamp when the hold started, in ISO 8601 format with an explicit offset. | `"2026-08-11T07:15:00+02:00"` |
| `end` | string or null | Timestamp when the hold was resolved, in ISO 8601 format with an explicit offset. | `"2026-08-12T11:00:00+02:00"` |
| `disposition` | string or null | Outcome assigned when the hold is resolved. | `"downgrade"` |
| `recoveredUnitValue` | number or null | Unit value recovered when the disposition retains some commercial value. | `0.41` |

</div>

## Hold lifecycle

A hold can be submitted as soon as the affected lot is withheld:

```json
{
  "code": "HOLD-2211",
  "lot": "FG-260810-113",
  "reason": "Metal detector challenge failed",
  "quantity": 4200,
  "unit": "pcs",
  "start": "2026-08-11T07:15:00+02:00"
}
```

When a decision is made, submit the same `code` with the fields that became known:

```json
{
  "code": "HOLD-2211",
  "disposition": "downgrade",
  "quantity": 4200,
  "recoveredUnitValue": 0.41,
  "end": "2026-08-12T11:00:00+02:00"
}
```

Fields omitted from the second request remain unchanged.

The time between `start` and `end` represents how long the lot remained on hold.

## Disposition

A resolved hold uses one of six dispositions:

| Disposition | Meaning |
| --- | --- |
| `release` | Return the held quantity to normal use. |
| `rework` | Send the quantity through additional processing. |
| `downgrade` | Retain the product at a lower grade or value. |
| `concession` | Accept the quantity under an approved concession. |
| `scrap` | Remove the quantity from usable production. |
| `destroy` | Destroy the affected quantity. |

These outcomes are kept separate because non-conforming product does not always represent a complete loss.

For example, off-spec product may be downgraded and sold at a lower value, or recovered through rework rather than being scrapped. :contentReference[oaicite:1]{index=1}

## Recovered value

When a disposition retains commercial value, `recoveredUnitValue` records the value known when the disposition decision is made.

For example:

```json
{
  "code": "HOLD-2211",
  "disposition": "downgrade",
  "quantity": 4200,
  "recoveredUnitValue": 0.41,
  "end": "2026-08-12T11:00:00+02:00"
}
```

Recording the recovered value allows Pulse to distinguish a partial loss from a complete loss.

The value belongs to the disposition decision rather than to the original hold because the recoverable value may not be known until the investigation is complete. :contentReference[oaicite:2]{index=2}

## Why hold duration matters

A hold is treated as work with a beginning and an end rather than as a single quality event.

While a lot is on hold, stock is unavailable and quality work is required to reach a decision.

Pulse can therefore compare hold duration across products, causes, lines, or other production context and identify situations where quality deviations take unusually long to resolve.

## API resource

| Resource | Base path |
| --- | --- |
| Hold | `/services/pulse/food-beverage/holds` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Lot](../master-data/lot.md)
- [Reason](../master-data/reason.md)

The referenced lot and reason must be available before submitting the hold.