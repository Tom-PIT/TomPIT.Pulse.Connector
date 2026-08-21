# API reference

The **Pulse API** is a REST API for submitting operational data and retrieving Pulse results.

This page provides an index of the public API families and services. Use [Scalar](https://scalar.com/) for the complete operation-level reference, including request fields, parameters, schemas, responses, and the exact operations supported by each service.

<div class="grid cards" markdown>

- [**Open the complete API reference in Scalar**](#)

</div>

## API families

<div class="grid cards" markdown>

- [**Master data**](#master-data)
- [**Manufacturing**](#manufacturing)
- [**Maintenance**](#maintenance)
- [**Answers**](#answers)

</div>

## Using this reference

Use this page to identify the relevant API family and service.

Open the service in Scalar to inspect:

- Available operations.
- Request fields and data types.
- Query parameters.
- Allowed values.
- Response schemas.
- Example requests, when available.

Use [Scalar](https://scalar.com/) to inspect and test individual requests. Use integration code for continuous or high-volume data exchange.

## API conventions

The Food & Beverage facade uses domain-shaped resources and business codes.

### Base route

Food & Beverage resources use the following base route:

```text
{host}/services/pulse/food-beverage/{resource}
```

For example:

```text
/services/pulse/food-beverage/products
/services/pulse/food-beverage/runs
/services/pulse/food-beverage/readings
```

### Business codes

Integrator-facing requests use business codes rather than Pulse numeric identifiers.

For example:

```json
{
  "code": "YOG-RUN-001",
  "line": "YOGURT-LINE-01",
  "product": "YOG-STRAWBERRY-150G"
}
```

Pulse may return an internal `id` in a response for support or log correlation, but Food & Beverage API requests do not use that `id` as an input.

### Timestamps

Timestamps sent to the API must use ISO 8601 format with an explicit UTC offset.

For example:

```text
2026-08-10T22:00:00+02:00
```

The API does not assume a local time zone.

### Partial updates

Lifecycle and other code-addressed resources support partial updates.

A field that is omitted means that its current value is unchanged.

An explicit `null` clears the value when the field supports clearing.

For example, a run can be submitted when it starts:

```json
{
  "code": "YOG-RUN-001",
  "line": "YOGURT-LINE-01",
  "product": "YOG-STRAWBERRY-150G",
  "at": "2026-08-10T22:00:00+02:00"
}
```

and submitted again later when its end is known:

```json
{
  "code": "YOG-RUN-001",
  "end": "2026-08-11T06:00:00+02:00",
  "status": "completed"
}
```

### Idempotency and corrections

Writes are idempotent on a key already owned by the source system.

For master-data and lifecycle resources, this is normally the record's `code`.

Replaying the same key with the same values does not create a duplicate.

Submitting the same key with corrected values supersedes the previous version rather than overwriting it.

### Stream resources

Stream resources such as readings, output, consumption, and line states can accept repeated operational records.

Stream endpoints accept either a single object or an array of objects.

When an array contains both accepted and rejected records, the API can return partial success with a result for each submitted item.

### Retraction

Correction and retraction are different operations.

A correction submits the same record key with updated values.

A retraction indicates that the record should not exist and is performed using `DELETE`.

Code-addressed resources can be retracted by their `code`. Stream records are retracted using the fields that form their record key.

### Validation without writing

Supported write resources can be validated without changing data by using:

```text
?dryRun=true
```

This validates the submitted payload and reports errors without persisting the record.

### Errors

API errors use stable error codes and include details about the field or relationship that caused the problem.

For example:

```json
{
  "error": "unknown-entity",
  "detail": "no line with code 'L07'",
  "path": "line"
}
```

See the complete API reference in Scalar for operation-specific schemas, allowed values, responses, and errors.

## Master data

Create and maintain the relatively stable business records referenced by Food & Beverage operational data.

All Food & Beverage master-data resources are addressed by business codes rather than Pulse numeric identifiers.

| Resource | Description | Base path |
| --- | --- | --- |
| [**Plant**](../master-data/plant.md) | Register a physical operating location. | `/services/pulse/food-beverage/plants` |
| [**Production line**](../master-data/production-line.md) | Register a production line within a plant. | `/services/pulse/food-beverage/lines` |
| [**Machine**](../master-data/machine.md) | Register a machine, component, or wear part associated with a production line. | `/services/pulse/food-beverage/machines` |
| [**Vessel**](../master-data/vessel.md) | Register a tank, silo, or other process vessel associated with a production line. | `/services/pulse/food-beverage/vessels` |
| [**Product**](../master-data/product.md) | Register a finished product or other output tracked in Pulse. | `/services/pulse/food-beverage/products` |
| [**Recipe**](../master-data/recipe.md) | Register a formulation version used during production. | `/services/pulse/food-beverage/recipes` |
| [**Material**](../master-data/material.md) | Register an ingredient, packaging material, chemical, or other material used in production. | `/services/pulse/food-beverage/materials` |
| [**Supplier**](../master-data/supplier.md) | Register a supplier associated with materials and other inputs. | `/services/pulse/food-beverage/suppliers` |
| [**Customer**](../master-data/customer.md) | Register a customer associated with Food & Beverage operations. | `/services/pulse/food-beverage/customers` |
| [**Shift**](../master-data/shift.md) | Register a work period used as production context. | `/services/pulse/food-beverage/shifts` |
| [**Crew**](../master-data/crew.md) | Register a team or operator group used to attribute work and labor consumption. | `/services/pulse/food-beverage/crews` |
| [**Lot**](../master-data/lot.md) | Register a traceable quantity of material received or produced. | `/services/pulse/food-beverage/lots` |
| [**Clean regime**](../master-data/clean-regime.md) | Register a cleaning regime such as dry clean, wet clean, full CIP, or allergen clean. | `/services/pulse/food-beverage/clean-regimes` |
| [**Reason**](../master-data/reason.md) | Register hierarchical causes used by stoppages, maintenance, holds, complaints, and other operational records. | `/services/pulse/food-beverage/reasons` |
| [**Metric**](../master-data/metric.md) | Declare a measurable or commanded signal used by readings and expected values. | `/services/pulse/food-beverage/metrics` |
| [**Types and attributes**](../master-data/types-and-attributes.md) | Declare analysable classifications and understand how they differ from additional source-system metadata. | `/services/pulse/food-beverage/types` |

### Lot analysis

Measured properties associated with a lot, such as fat, protein, moisture, or other composition values, are submitted through the lot analysis resource:

`POST /services/pulse/food-beverage/lots/{code}/analysis`

See [Lot](../master-data/lot.md) for details.

## Manufacturing

Submit Food & Beverage production work, resource consumption, output, measurements, line conditions, and operational events.

### Production work

| Resource | Description | Base path |
| --- | --- | --- |
| [**Run**](../manufacturing/run.md) | Submit a production episode for a product on a production line. | `/services/pulse/food-beverage/runs` |
| [**Batch**](../manufacturing/batch.md) | Submit a process batch such as a cook, mix, fermentation, or other bulk-production step. | `/services/pulse/food-beverage/batches` |
| [**Stage**](../manufacturing/stage.md) | Submit an execution step within a production run. | `/services/pulse/food-beverage/stages` |
| [**Clean**](../manufacturing/clean.md) | Submit a cleaning activity on a production line. | `/services/pulse/food-beverage/cleans` |
| [**Hold**](../manufacturing/hold.md) | Submit a quality hold placed on a specific lot. | `/services/pulse/food-beverage/holds` |

### Operational records

| Resource | Description | Base path |
| --- | --- | --- |
| [**Consumption**](../manufacturing/consumption.md) | Submit actual ingredients, packaging, chemicals, utilities, labor, equipment, and other resources consumed by work. | `/services/pulse/food-beverage/consumption` |
| [**Output**](../manufacturing/output.md) | Submit good output, waste, downgrade, and reject quantities produced during a run. | `/services/pulse/food-beverage/output` |
| [**Reading**](../manufacturing/reading.md) | Submit measured or commanded values associated with production entities and activities. | `/services/pulse/food-beverage/readings` |
| [**Line state**](../manufacturing/line-state.md) | Submit non-running or constrained production-line intervals for time accounting. | `/services/pulse/food-beverage/lines/{code}/states` |
| [**Event**](../manufacturing/event.md) | Submit discrete operational occurrences such as stoppages, deviations, waste, rework, or rejects. | `/services/pulse/food-beverage/events` |

### Run plans

Planned production quantity and planned resource items can be submitted as part of the Run object or separately through:

`POST /services/pulse/food-beverage/runs/{code}/plan`

See [Run](../manufacturing/run.md) for details.

### Resource plans

| Service | Description | Base path |
| --- | --- | --- |
| [**Energy source plan**](../manufacturing/energy-source-plan.md)<br>`EnergySourcePlanService` | Describe planned energy quantity and price for a stage. | `/services/pulse/manufacturing/batches/stages/plan/energy-sources` |
| [**Equipment plan**](../manufacturing/equipment-plan.md)<br>`EquipmentPlanService` | Describe planned equipment hours and hourly price for a stage. | `/services/pulse/manufacturing/batches/stages/plan/equipment` |
| [**Equipment plan period**](../manufacturing/equipment-plan-period.md)<br>`EquipmentPlanPeriodService` | Describe a specific planned equipment-use interval. | `/services/pulse/manufacturing/batches/stages/plan/equipment/periods` |
| [**Expense plan**](../manufacturing/expense-plan.md)<br>`ExpensePlanService` | Describe an additional cost planned for a stage. | `/services/pulse/manufacturing/batches/stages/plan/expenses` |
| [**Labor plan**](../manufacturing/labor-plan.md)<br>`LaborPlanService` | Describe planned labor hours and hourly price for a stage. | `/services/pulse/manufacturing/batches/stages/plan/labor` |
| [**Material plan**](../manufacturing/material-plan.md)<br>`MaterialPlanService` | Describe planned material quantity and price for a stage. | `/services/pulse/manufacturing/batches/stages/plan/materials` |

### Resource usage

| Service | Description | Base path |
| --- | --- | --- |
| [**Energy source usage**](../manufacturing/energy-source-usage.md)<br>`EnergySourceUsageService` | Record actual energy quantity and price for a stage. | `/services/pulse/manufacturing/batches/stages/usage/energy-sources` |
| [**Equipment usage**](../manufacturing/equipment-usage.md)<br>`EquipmentUsageService` | Record actual equipment hours and hourly price for a stage. | `/services/pulse/manufacturing/batches/stages/usage/equipment` |
| [**Equipment usage period**](../manufacturing/equipment-usage-period.md)<br>`EquipmentUsagePeriodService` | Record a specific actual equipment-use interval. | `/services/pulse/manufacturing/batches/stages/usage/equipment/periods` |
| [**Expense usage**](../manufacturing/expense-usage.md)<br>`ExpenseUsageService` | Record an additional cost incurred during a stage. | `/services/pulse/manufacturing/batches/stages/usage/expenses` |
| [**Labor usage**](../manufacturing/labor-usage.md)<br>`LaborUsageService` | Record actual labor hours and hourly price for a stage. | `/services/pulse/manufacturing/batches/stages/usage/labor` |
| [**Labor usage period**](../manufacturing/labor-usage-period.md)<br>`LaborUsagePeriodService` | Record a specific interval during which labor was performed. | `/services/pulse/manufacturing/batches/stages/usage/labor/periods` |
| [**Material usage**](../manufacturing/material-usage.md)<br>`MaterialUsageService` | Record actual material quantity and price for a stage. | `/services/pulse/manufacturing/batches/stages/usage/materials` |

### Downtime

| Service | Description | Base path |
| --- | --- | --- |
| [**Downtime**](../manufacturing/downtime.md)<br>`DowntimeService` | Record a downtime event associated with a stage. | `/services/pulse/manufacturing/batches/stages/downtime` |
| [**Downtime maintenance**](../manufacturing/downtime-maintenance.md)<br>`DowntimeMaintenanceService` | Link a downtime event to a maintenance activity. | `/services/pulse/manufacturing/batches/stages/downtime/maintenance` |
| [**Downtime plan**](../manufacturing/downtime-plan.md)<br>`DowntimePlanService` | Describe the planned downtime interval. | `/services/pulse/manufacturing/batches/stages/downtime/plan` |
| [**Downtime usage**](../manufacturing/downtime-usage.md)<br>`DowntimeUsageService` | Record the actual downtime interval. | `/services/pulse/manufacturing/batches/stages/downtime/usage` |

### Waste

| Service | Description | Base path |
| --- | --- | --- |
| [**Waste**](../manufacturing/waste.md)<br>`WasteService` | Record waste, scrap, loss, or another unusable quantity. | `/services/pulse/manufacturing/batches/stages/usage/waste` |
| [**Waste energy source usage**](../manufacturing/waste-energy-source-usage.md)<br>`WasteEnergySourceUsageService` | Record energy attributed to waste. | `/services/pulse/manufacturing/batches/stages/usage/waste/energy-sources` |
| [**Waste expense usage**](../manufacturing/waste-expense-usage.md)<br>`WasteExpenseUsageService` | Record additional expenses attributed to waste. | `/services/pulse/manufacturing/batches/stages/usage/waste/expenses` |
| [**Waste material usage**](../manufacturing/waste-material-usage.md)<br>`WasteMaterialUsageService` | Record material attributed to waste. | `/services/pulse/manufacturing/batches/stages/usage/waste/materials` |

## Maintenance

Submit preventive and corrective maintenance activities, planned requirements, actual timing, resource usage, and costs.

### Core records

| Service | Description | Base path |
| --- | --- | --- |
| [**Maintenance**](../maintenance/maintenance.md)<br>`MaintenanceService` | Identify and classify a preventive or corrective maintenance activity. | `/services/pulse/maintenance` |
| [**Maintenance plan**](../maintenance/maintenance-plan.md)<br>`MaintenancePlanService` | Describe the planned timing of a maintenance activity. | `/services/pulse/maintenance/plan` |
| [**Maintenance usage**](../maintenance/maintenance-usage.md)<br>`MaintenanceUsageService` | Describe the actual timing of a maintenance activity. | `/services/pulse/maintenance/usage` |

### Resource plans

| Service | Description | Base path |
| --- | --- | --- |
| [**Maintenance energy source plan**](../maintenance/maintenance-energy-source-plan.md)<br>`MaintenanceEnergySourcePlanService` | Describe planned energy quantity and price for a maintenance activity. | `/services/pulse/maintenance/estimations/energy-sources` |
| [**Maintenance equipment plan**](../maintenance/maintenance-equipment-plan.md)<br>`MaintenanceEquipmentPlanService` | Describe planned equipment hours and hourly price for a maintenance activity. | `/services/pulse/maintenance/plan/equipment` |
| [**Maintenance expense plan**](../maintenance/maintenance-expense-plan.md)<br>`MaintenanceExpensePlanService` | Describe an additional cost planned for a maintenance activity. | `/services/pulse/maintenance/plan/expenses` |
| [**Maintenance labor plan**](../maintenance/maintenance-labor-plan.md)<br>`MaintenanceLaborPlanService` | Describe planned labor hours and hourly price for a maintenance activity. | `/services/pulse/maintenance/plan/labor` |
| [**Maintenance material plan**](../maintenance/maintenance-material-plan.md)<br>`MaintenanceMaterialPlanService` | Describe planned material quantity and price for a maintenance activity. | `/services/pulse/maintenance/plan/materials` |

### Resource usage

| Service | Description | Base path |
| --- | --- | --- |
| [**Maintenance energy source usage**](../maintenance/maintenance-energy-source-usage.md)<br>`MaintenanceEnergySourceUsageService` | Record actual energy quantity and price for a maintenance activity. | `/services/pulse/maintenance/usage/energy-sources` |
| [**Maintenance equipment usage**](../maintenance/maintenance-equipment-usage.md)<br>`MaintenanceEquipmentUsageService` | Record actual equipment hours and hourly price for a maintenance activity. | `/services/pulse/maintenance/usage/equipment` |
| [**Maintenance expense usage**](../maintenance/maintenance-expense-usage.md)<br>`MaintenanceExpenseUsageService` | Record an additional cost incurred during a maintenance activity. | `/services/pulse/maintenance/usage/expenses` |
| [**Maintenance labor usage**](../maintenance/maintenance-labor-usage.md)<br>`MaintenanceLaborUsageService` | Record actual labor hours and hourly price for a maintenance activity. | `/services/pulse/maintenance/usage/labor` |
| [**Maintenance material usage**](../maintenance/maintenance-material-usage.md)<br>`MaintenanceMaterialUsageService` | Record actual material quantity and price for a maintenance activity. | `/services/pulse/maintenance/usage/materials` |

## Answers

Retrieve analytical results, insights, recommendations, forecasts, and improvement actions.

| Service | Description | Base path |
| --- | --- | --- |
| **Assessment**<br>`AssessmentService` | Retrieve an assessment produced by Pulse analysis. | `/services/pulse/analysis/batches/assessment` |
| **Assessment content**<br>`AssessmentContentService` | Retrieve assessment content produced by Pulse analysis. | `/services/pulse/analysis/batches/assessment/content` |
| **Conclusion**<br>`ConclusionService` | Retrieve a conclusion produced by Pulse analysis. | `/services/pulse/analysis/batches/assessment/measurements` |
| **Conclusion content**<br>`ConclusionContentService` | Retrieve conclusion content produced by Pulse analysis. | `/services/pulse/analysis/batches/assessment/measurements/content` |
| **Forecast**<br>`ForecastService` | Retrieve a forecast produced by Pulse analysis. | `/services/pulse/analysis/forecasts` |
| **Forecast content**<br>`ForecastContentService` | Retrieve forecast content produced by Pulse analysis. | `/services/pulse/analysis/forecasts/content` |
| **Improvement action**<br>`ImprovementActionService` | Retrieve an improvement action produced by Pulse analysis. | `/services/pulse/analysis/improvements/actions` |
| **Improvement action content**<br>`ImprovementActionContentService` | Retrieve improvement action content produced by Pulse analysis. | `/services/pulse/analysis/improvements/actions/content` |
| **Insight**<br>`InsightService` | Retrieve an insight produced by Pulse analysis. | `/services/pulse/analysis/improvements/insights` |
| **Insight content**<br>`InsightContentService` | Retrieve insight content produced by Pulse analysis. | `/services/pulse/analysis/improvements/insights/content` |
| **Recommendation**<br>`RecommendationService` | Retrieve a recommendation produced by Pulse analysis. | `/services/pulse/analysis/improvements/recommendations` |
| **Recommendation content**<br>`RecommendationContentService` | Retrieve recommendation content produced by Pulse analysis. | `/services/pulse/analysis/improvements/recommendations/content` |
| **Recommendation scope**<br>`RecommendationScopeService` | Retrieve a recommendation scope produced by Pulse analysis. | `/services/pulse/analysis/improvements/scopes` |