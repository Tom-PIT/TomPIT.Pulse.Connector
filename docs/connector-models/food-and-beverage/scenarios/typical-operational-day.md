# End-to-end operational scenario

This scenario shows how a Food and Beverage integration can submit one complete operational flow to Pulse.

The exact records depend on the source system and the operation being represented. Submit only the records that apply.

## Scenario

A planned operational batch runs on a production line. It contains two stages, consumes materials and energy, uses equipment and labor, produces output, records a downtime event, creates waste, and includes measured values.

## 1. Synchronize master data

Create or retrieve the required records:

- [Plant](../master-data/plant.md)
- [Production line](../master-data/production-line.md)
- [Product](../master-data/product.md)
- [Measure units](../master-data/measure-unit.md)
- [Materials](../master-data/material.md)
- [Energy sources](../master-data/energy-source.md)
- [Equipment](../master-data/equipment.md)
- [Labor](../master-data/labor.md)
- [Shifts](../master-data/shift.md)
- [Downtime category](../master-data/downtime-category.md)
- [Waste types](../master-data/waste-type.md)
- [Ambient types](../master-data/ambient-type.md)

Retrieve code-based records by `code` and use the returned Pulse `id` values.

## 2. Create the batch

Create a [Batch](../manufacturing/batch.md) for the operational unit of work.

```json
{
  "code": "BATCH-2026-0717-01"
}
```

Store the returned `id` for the current workflow.

## 3. Submit batch plan and usage

Use the Batch `id` for both records.

```text
Batch.id = BatchPlan.id = BatchUsage.id
```

Submit the planned timing and quantity through [Batch plan](../manufacturing/batch-plan.md).

Submit the actual timing through [Batch usage](../manufacturing/batch-usage.md).

Assign applicable shifts through [Batch shift](../manufacturing/batch-shift.md).

## 4. Create the stages

Create the stages that belong to the Batch.

For each [Stage](../manufacturing/stage.md):

1. Submit its planned timing.
2. Submit its actual timing.
3. Submit applicable resource plans.
4. Submit actual resource usage.

Use the Stage `id` for both Stage plan and Stage usage.

## 5. Submit resource plans

Submit expected resources for each Stage:

- [Material plan](../manufacturing/material-plan.md).
- [Energy source plan](../manufacturing/energy-source-plan.md).
- [Equipment plan](../manufacturing/equipment-plan.md).
- [Labor plan](../manufacturing/labor-plan.md).
- [Expense plan](../manufacturing/expense-plan.md).

Equipment and Labor quantities represent hours. Their time-based prices are expressed per hour.

## 6. Submit actual usage

Submit what was actually consumed or used:

- [Material usage](../manufacturing/material-usage.md).
- [Energy source usage](../manufacturing/energy-source-usage.md).
- [Equipment usage](../manufacturing/equipment-usage.md).
- [Labor usage](../manufacturing/labor-usage.md).
- [Expense usage](../manufacturing/expense-usage.md).

Use the related Stage `id`.

## 7. Record output

Submit [Produced](../manufacturing/produced.md) records for output quantities and quality classification.

Bad-quality output is still Produced. Use Waste for scrap, loss, or unusable quantity.

## 8. Record downtime and maintenance

When a Stage is interrupted:

1. Create a [Downtime](../manufacturing/downtime.md) record.
2. Submit its planned or actual timing as applicable.
3. Create a [Maintenance](../maintenance/maintenance.md) record when maintenance work is performed.
4. Submit maintenance plan, usage, and resources.
5. Link the records through [Downtime maintenance](../manufacturing/downtime-maintenance.md).

Submit the attributed percentage as a decimal fraction. For example:

```json
{
  "percentage": 0.25
}
```

## 9. Record waste

Create a [Waste](../manufacturing/waste.md) record when the operation generates scrap, loss, or another unusable quantity.

Submit related detail records when applicable:

- Waste material usage.
- Waste energy source usage.
- Waste expense usage.

## 10. Submit measurements

Submit operational measurements as [Ambient value](../manufacturing/ambient-value.md) records.

Use the most precise [Dimension](../data-model/dimension.md) available, such as Stage, Equipment, or Batch.

```json
{
  "dimension": 3,
  "dimensionId": 208,
  "value": 72.4
}
```

## 11. Verify the submitted data

After each step:

- Inspect the response.
- Confirm referenced records exist.
- Retrieve important records by `code` when a current Pulse `id` is required.
- Log failures and retry only after identifying the cause.

See [Validation](../../../integration/validation.md) and [Updates and error handling](../../../integration/updates-and-error-handling.md).
