# End-to-end operational scenario

This scenario shows how a Food & Beverage integration can submit one complete production flow to Pulse.

The exact records depend on the source system and production process. Submit only the records that apply.

## Scenario

A batch of strawberry yogurt is produced on a yogurt production line.

The batch moves through production stages that prepare and process the product before filling and packaging. The process consumes ingredients and energy, uses production equipment and labor, produces finished output, records downtime and waste when they occur, and captures process measurements such as temperature.

The example demonstrates how these records are connected in Pulse.

## 1. Synchronize master data

Create or retrieve the required records.

For this scenario, they may include:

- [Plant](../master-data/plant.md), such as a dairy production plant.
- [Production line](../master-data/production-line.md), such as a yogurt production line.
- [Product](../master-data/product.md), such as strawberry yogurt.
- [Measure units](../master-data/measure-unit.md), such as kilograms, liters, and degrees Celsius.
- [Materials](../master-data/material.md), such as milk, fruit preparation, sugar, and packaging material.
- [Energy sources](../master-data/energy-source.md).
- [Machine](../master-data/machine.md), such as a pasteurizer, mixing tank, or filling machine.
- [Labor](../master-data/labor.md).
- [Shifts](../master-data/shift.md).
- [Downtime category](../master-data/downtime-category.md).
- [Waste types](../master-data/waste-type.md), such as product loss or packaging waste.
- [Ambient types](../master-data/ambient-type.md), such as product temperature.

Retrieve code-based records by `code` and use the returned Pulse `id` values.

## 2. Create the batch

Create a [Batch](../manufacturing/batch.md) for the yogurt production batch.

```json
{
  "code": "YOG-2026-0717-01"
}
```

Store the returned `id` for the current workflow.

The Batch connects the production activity to the applicable Product, Production line, and other related records.

## 3. Submit batch plan and usage

Use the Batch `id` for both records.

```text
Batch.id = BatchPlan.id = BatchUsage.id
```

Submit the planned production timing and quantity through [Batch plan](../manufacturing/batch-plan.md).

Submit the actual production timing through [Batch usage](../manufacturing/batch-usage.md).

Assign applicable shifts through [Batch shift](../manufacturing/batch-shift.md).

## 4. Create the production stages

Create the [Stages](../manufacturing/stage.md) that belong to the Batch.

For example, the yogurt production process may contain stages such as:

1. Mixing and pasteurization.
2. Filling and packaging.

For each Stage:

1. Submit its planned timing.
2. Submit its actual timing.
3. Submit applicable resource plans.
4. Submit actual resource usage.

Use the Stage `id` for both Stage plan and Stage usage.

## 5. Submit resource plans

Submit the resources expected for each Stage.

Depending on the production process, this may include:

- [Material plan](../manufacturing/material-plan.md) for ingredients or packaging material.
- [Energy source plan](../manufacturing/energy-source-plan.md) for expected energy consumption.
- [Equipment plan](../manufacturing/equipment-plan.md) for production equipment.
- [Labor plan](../manufacturing/labor-plan.md) for expected labor.
- [Expense plan](../manufacturing/expense-plan.md) for other planned costs.

For example, the mixing and pasteurization stage may plan milk, fruit preparation, energy, a mixing tank, a pasteurizer, and the required labor.

Equipment and Labor quantities represent hours. Their time-based prices are expressed per hour.

## 6. Submit actual usage

Submit the resources actually consumed or used during production:

- [Material usage](../manufacturing/material-usage.md).
- [Energy source usage](../manufacturing/energy-source-usage.md).
- [Equipment usage](../manufacturing/equipment-usage.md).
- [Labor usage](../manufacturing/labor-usage.md).
- [Expense usage](../manufacturing/expense-usage.md).

Use the related Stage `id`.

For example, Material usage can record the actual quantities of milk, fruit preparation, or packaging material consumed during the applicable production stage.

When lot traceability applies, associate resource usage with the applicable [Lot](../traceability/lot.md).

## 7. Record produced output

Submit [Produced](../manufacturing/produced.md) records for the quantities produced and their quality classification.

For example, the filling and packaging stage may record the quantity of finished yogurt produced.

Bad-quality output is still Produced. Use Waste for scrap, product loss, packaging loss, or another unusable quantity.

## 8. Record downtime and maintenance

When a production Stage is interrupted:

1. Create a [Downtime](../manufacturing/downtime.md) record.
2. Submit its planned or actual timing as applicable.
3. Create a [Maintenance](../maintenance/maintenance.md) record when maintenance work is performed.
4. Submit maintenance plan, usage, and resources.
5. Link the records through [Downtime maintenance](../manufacturing/downtime-maintenance.md).

For example, downtime may occur because a filling machine stops and requires corrective maintenance.

Submit the attributed percentage as a decimal fraction. For example:

```json
{
  "percentage": 0.25
}
```

## 9. Record waste

Create a [Waste](../manufacturing/waste.md) record when the process generates product loss, rejected material, packaging waste, or another unusable quantity.

Examples can include:

- Product lost during filling.
- Rejected product.
- Ingredient loss.
- Damaged packaging material.

Submit related detail records when applicable:

- Waste material usage.
- Waste energy source usage.
- Waste expense usage.

## 10. Submit measurements

Submit production and process measurements as [Ambient value](../manufacturing/ambient-value.md) records.

Food & Beverage processes commonly depend on measurements such as temperature, pressure, humidity, or other process and environmental values.

For example, the integration may submit the measured product temperature during pasteurization.

Use the most precise [Dimension](../data-model/dimension.md) available, such as Stage, Equipment, Batch, or Production line.

```json
{
  "dimension": 3,
  "dimensionId": 208,
  "value": 72.4
}
```

Here, the value could represent a temperature measurement associated with a specific production Stage.

## 11. Verify the submitted data

After each step:

- Inspect the response.
- Confirm referenced records exist.
- Retrieve important records by `code` when a current Pulse `id` is required.
- Confirm that plans, actual usage, output, waste, downtime, and measurements reference the intended production records.
- Log failures and retry only after identifying the cause.

See [Validation](../../../../integration/validation.md) and [Updates and error handling](../../../../integration/updates-and-error-handling.md).
