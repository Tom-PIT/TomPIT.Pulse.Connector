# Waste

Represents waste, scrap, loss, or another unusable quantity recorded during a stage.

A waste record identifies where and when the loss occurred. It may also classify the loss through a [waste type](../master-data/waste-type.md). For example, a Waste record can represent product lost during processing, filling, or packaging.

> [!IMPORTANT]
> Bad-quality completed items are still recorded as [produced output](produced.md). Use Waste for material, energy, expense, or other value lost during execution, not solely because a produced item has bad quality.

## The Waste object

```json
{
  "id": 812,
  "stage": 208,
  "date": "2026-07-17T09:42:00+02:00",
  "type": 16
}
```

## Attributes

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `id` | integer | Unique identifier of the waste record. | `812` |
| [`stage`](stage.md) | integer | Pulse `id` of the stage in which the waste was recorded. | `208` |
| `date` | string | Date and time when the waste occurred or was recorded, in ISO 8601 format. | `"2026-07-17T09:42:00+02:00"` |
| [`type`](../master-data/waste-type.md) | integer or null | Optional Pulse `id` of the waste type. | `16` |

</div>

Related usage records describe the material, energy, and additional expenses attributed to the waste.

## API service

| Service | Base path |
| --- | --- |
| `WasteService` | `/services/pulse/manufacturing/batches/stages/usage/waste` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

- [Stage](stage.md)

May also reference:

- [Waste type](../master-data/waste-type.md)

Create or retrieve the applicable records before submitting the waste record.

## Referenced by

- [Waste material usage records](waste-material-usage.md)
- [Waste energy source usage records](waste-energy-source-usage.md)
- [Waste expense usage records](waste-expense-usage.md)