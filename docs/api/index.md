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

Use Scalar to inspect and test individual requests. Use integration code for continuous or high-volume data exchange.

## API conventions

Many Pulse services expose some or all of the following operations. The exact operation set and behavior depend on the service.

| Operation | HTTP method | Purpose |
| --- | --- | --- |
| `insert` | `POST` | Create a record. |
| `update` | `PUT` | Replace the complete editable representation of an existing record. |
| `patch` | `PATCH` | Update only the specified attributes. |
| `delete` | `DELETE` | Delete a record. |
| `select` | `GET` | Retrieve one record by its Pulse `id`. |
| `query` | `GET` | Retrieve and filter multiple records. |

Insert operations commonly return the Pulse `id` assigned to the new record.

When an integration later needs the current Pulse `id` of a code-based record, retrieve the record by `code` and use the returned `id`.

## Master data

Create and maintain the stable business entities and code lists referenced by operational records.

| Service | Description | Base path |
| --- | --- | --- |
| [**Ambient type**](../integration/master-data/ambient-type.md)<br>`AmbientTypeService` | Define measurement types, units, and expected value ranges. | `/services/pulse/types/ambient-types` |
| [**Customer**](../integration/master-data/customer.md)<br>`CustomerService` | Define customers referenced by operational records. | `/services/pulse/types/customers` |
| [**Delay**](../integration/master-data/delay.md)<br>`DelayService` | Define delay classifications used by stage delay records. | `/services/pulse/types/delays` |
| [**Downtime category**](../integration/master-data/downtime-category.md)<br>`DowntimeCategoryService` | Define categories used to group downtime types. | `/services/pulse/types/downtime-categories` |
| [**Downtime cause**](../integration/master-data/downtime-cause.md)<br>`DowntimeCauseService` | Define causes associated with downtime records. | `/services/pulse/types/downtime-causes` |
| [**Downtime type**](../integration/master-data/downtime-type.md)<br>`DowntimeTypeService` | Define planned or unplanned downtime types. | `/services/pulse/types/downtime-types` |
| [**Energy source**](../integration/master-data/energy-source.md)<br>`EnergySourceService` | Define energy sources and their default price per measure unit. | `/services/pulse/types/energy-sources` |
| [**Equipment**](../integration/master-data/equipment.md)<br>`EquipmentService` | Define equipment resources used during operational activities. | `/services/pulse/types/equipment` |
| [**Expense**](../integration/master-data/expense.md)<br>`ExpenseService` | Define additional cost types used by operational records. | `/services/pulse/types/expenses` |
| [**Labor**](../integration/master-data/labor.md)<br>`LaborService` | Define labor categories or resources used during activities. | `/services/pulse/types/labor` |
| [**Maintenance reason**](../integration/master-data/maintenance-reason.md)<br>`MaintenanceReasonService` | Define reasons used to classify maintenance activities. | `/services/pulse/types/maintenance-reasons` |
| [**Material**](../integration/master-data/material.md)<br>`MaterialService` | Define raw materials, components, or supplies consumed during activities. | `/services/pulse/types/materials` |
| [**Measure unit**](../integration/master-data/measure-unit.md)<br>`MeasureUnitService` | Define units used for quantities, measurements, and prices. | `/services/pulse/types/measure-units` |
| [**Plant**](../integration/master-data/plant.md)<br>`PlantService` | Define operating locations referenced by production lines. | `/services/pulse/types/plants` |
| [**Product**](../integration/master-data/product.md)<br>`ProductService` | Define finished products or other outputs tracked in Pulse. | `/services/pulse/types/products` |
| [**Production line**](../integration/master-data/production-line.md)<br>`ProductionLineService` | Define production lines associated with plants. | `/services/pulse/types/production-lines` |
| [**Shift**](../integration/master-data/shift.md)<br>`ShiftService` | Define work shifts referenced by operational records. | `/services/pulse/types/shifts` |
| [**Supplier**](../integration/master-data/supplier.md)<br>`SupplierService` | Define suppliers referenced by material or energy usage. | `/services/pulse/types/suppliers` |
| [**Waste type**](../integration/master-data/waste-type.md)<br>`WasteTypeService` | Define classifications used by waste records. | `/services/pulse/types/waste-types` |

## Manufacturing

Submit planned and actual operational activity, resource usage, output, downtime, waste, and measurements.

### Core records

| Service | Description | Base path |
| --- | --- | --- |
| [**Ambient value**](../integration/manufacturing/ambient-value.md)<br>`AmbientValueService` | Submit and retrieve measured values linked to Pulse entities. | `/services/pulse/manufacturing/ambient-values` |
| [**Batch**](../integration/manufacturing/batch.md)<br>`BatchService` | Manage operational batches and their business context. | `/services/pulse/manufacturing/batches` |
| [**Batch plan**](../integration/manufacturing/batch-plan.md)<br>`BatchPlanService` | Describe planned batch timing and quantity. | `/services/pulse/manufacturing/batches/plan` |
| [**Batch shift**](../integration/manufacturing/batch-shift.md)<br>`BatchShiftService` | Assign shifts to a batch during specific intervals. | `/services/pulse/manufacturing/batches/shifts` |
| [**Batch usage**](../integration/manufacturing/batch-usage.md)<br>`BatchUsageService` | Record actual batch timing. | `/services/pulse/manufacturing/batches/usage` |
| [**Produced**](../integration/manufacturing/produced.md)<br>`ProducedService` | Record output quantities and quality classification. | `/services/pulse/manufacturing/batches/produced` |
| [**Stage**](../integration/manufacturing/stage.md)<br>`StageService` | Manage operations or execution steps within a batch. | `/services/pulse/manufacturing/batches/stages` |
| [**Stage plan**](../integration/manufacturing/stage-plan.md)<br>`StagePlanService` | Describe planned stage timing. | `/services/pulse/manufacturing/batches/stages/plan` |
| [**Stage usage**](../integration/manufacturing/stage-usage.md)<br>`StageUsageService` | Record actual stage timing. | `/services/pulse/manufacturing/batches/stages/usage` |
| [**Stage delay**](../integration/manufacturing/stage-delay.md)<br>`StageDelayService` | Record a delay associated with a stage. | `/services/pulse/manufacturing/batches/stages/delays` |

### Resource plans

| Service | Description | Base path |
| --- | --- | --- |
| [**Energy source plan**](../integration/manufacturing/energy-source-plan.md)<br>`EnergySourcePlanService` | Describe planned energy quantity and price for a stage. | `/services/pulse/manufacturing/batches/stages/plan/energy-sources` |
| [**Equipment plan**](../integration/manufacturing/equipment-plan.md)<br>`EquipmentPlanService` | Describe planned equipment hours and hourly price for a stage. | `/services/pulse/manufacturing/batches/stages/plan/equipment` |
| [**Equipment plan period**](../integration/manufacturing/equipment-plan-period.md)<br>`EquipmentPlanPeriodService` | Describe a specific planned equipment-use interval. | `/services/pulse/manufacturing/batches/stages/plan/equipment/periods` |
| [**Expense plan**](../integration/manufacturing/expense-plan.md)<br>`ExpensePlanService` | Describe an additional cost planned for a stage. | `/services/pulse/manufacturing/batches/stages/plan/expenses` |
| [**Labor plan**](../integration/manufacturing/labor-plan.md)<br>`LaborPlanService` | Describe planned labor hours and hourly price for a stage. | `/services/pulse/manufacturing/batches/stages/plan/labor` |
| [**Material plan**](../integration/manufacturing/material-plan.md)<br>`MaterialPlanService` | Describe planned material quantity and price for a stage. | `/services/pulse/manufacturing/batches/stages/plan/materials` |

### Resource usage

| Service | Description | Base path |
| --- | --- | --- |
| [**Energy source usage**](../integration/manufacturing/energy-source-usage.md)<br>`EnergySourceUsageService` | Record actual energy quantity and price for a stage. | `/services/pulse/manufacturing/batches/stages/usage/energy-sources` |
| [**Equipment usage**](../integration/manufacturing/equipment-usage.md)<br>`EquipmentUsageService` | Record actual equipment hours and hourly price for a stage. | `/services/pulse/manufacturing/batches/stages/usage/equipment` |
| [**Equipment usage period**](../integration/manufacturing/equipment-usage-period.md)<br>`EquipmentUsagePeriodService` | Record a specific actual equipment-use interval. | `/services/pulse/manufacturing/batches/stages/usage/equipment/periods` |
| [**Expense usage**](../integration/manufacturing/expense-usage.md)<br>`ExpenseUsageService` | Record an additional cost incurred during a stage. | `/services/pulse/manufacturing/batches/stages/usage/expenses` |
| [**Labor usage**](../integration/manufacturing/labor-usage.md)<br>`LaborUsageService` | Record actual labor hours and hourly price for a stage. | `/services/pulse/manufacturing/batches/stages/usage/labor` |
| [**Labor usage period**](../integration/manufacturing/labor-usage-period.md)<br>`LaborUsagePeriodService` | Record a specific interval during which labor was performed. | `/services/pulse/manufacturing/batches/stages/usage/labor/periods` |
| [**Material usage**](../integration/manufacturing/material-usage.md)<br>`MaterialUsageService` | Record actual material quantity and price for a stage. | `/services/pulse/manufacturing/batches/stages/usage/materials` |

### Downtime

| Service | Description | Base path |
| --- | --- | --- |
| [**Downtime**](../integration/manufacturing/downtime.md)<br>`DowntimeService` | Record a downtime event associated with a stage. | `/services/pulse/manufacturing/batches/stages/downtime` |
| [**Downtime maintenance**](../integration/manufacturing/downtime-maintenance.md)<br>`DowntimeMaintenanceService` | Link a downtime event to a maintenance activity. | `/services/pulse/manufacturing/batches/stages/downtime/maintenance` |
| [**Downtime plan**](../integration/manufacturing/downtime-plan.md)<br>`DowntimePlanService` | Describe the planned downtime interval. | `/services/pulse/manufacturing/batches/stages/downtime/plan` |
| [**Downtime usage**](../integration/manufacturing/downtime-usage.md)<br>`DowntimeUsageService` | Record the actual downtime interval. | `/services/pulse/manufacturing/batches/stages/downtime/usage` |

### Waste

| Service | Description | Base path |
| --- | --- | --- |
| [**Waste**](../integration/manufacturing/waste.md)<br>`WasteService` | Record waste, scrap, loss, or another unusable quantity. | `/services/pulse/manufacturing/batches/stages/usage/waste` |
| [**Waste energy source usage**](../integration/manufacturing/waste-energy-source-usage.md)<br>`WasteEnergySourceUsageService` | Record energy attributed to waste. | `/services/pulse/manufacturing/batches/stages/usage/waste/energy-sources` |
| [**Waste expense usage**](../integration/manufacturing/waste-expense-usage.md)<br>`WasteExpenseUsageService` | Record additional expenses attributed to waste. | `/services/pulse/manufacturing/batches/stages/usage/waste/expenses` |
| [**Waste material usage**](../integration/manufacturing/waste-material-usage.md)<br>`WasteMaterialUsageService` | Record material attributed to waste. | `/services/pulse/manufacturing/batches/stages/usage/waste/materials` |

## Maintenance

Submit preventive and corrective maintenance activities, planned requirements, actual timing, resource usage, and costs.

### Core records

| Service | Description | Base path |
| --- | --- | --- |
| [**Maintenance**](../integration/maintenance/maintenance.md)<br>`MaintenanceService` | Identify and classify a preventive or corrective maintenance activity. | `/services/pulse/maintenance` |
| [**Maintenance plan**](../integration/maintenance/maintenance-plan.md)<br>`MaintenancePlanService` | Describe the planned timing of a maintenance activity. | `/services/pulse/maintenance/plan` |
| [**Maintenance usage**](../integration/maintenance/maintenance-usage.md)<br>`MaintenanceUsageService` | Describe the actual timing of a maintenance activity. | `/services/pulse/maintenance/usage` |

### Resource plans

| Service | Description | Base path |
| --- | --- | --- |
| [**Maintenance energy source plan**](../integration/maintenance/maintenance-energy-source-plan.md)<br>`MaintenanceEnergySourcePlanService` | Describe planned energy quantity and price for a maintenance activity. | `/services/pulse/maintenance/estimations/energy-sources` |
| [**Maintenance equipment plan**](../integration/maintenance/maintenance-equipment-plan.md)<br>`MaintenanceEquipmentPlanService` | Describe planned equipment hours and hourly price for a maintenance activity. | `/services/pulse/maintenance/plan/equipment` |
| [**Maintenance expense plan**](../integration/maintenance/maintenance-expense-plan.md)<br>`MaintenanceExpensePlanService` | Describe an additional cost planned for a maintenance activity. | `/services/pulse/maintenance/plan/expenses` |
| [**Maintenance labor plan**](../integration/maintenance/maintenance-labor-plan.md)<br>`MaintenanceLaborPlanService` | Describe planned labor hours and hourly price for a maintenance activity. | `/services/pulse/maintenance/plan/labor` |
| [**Maintenance material plan**](../integration/maintenance/maintenance-material-plan.md)<br>`MaintenanceMaterialPlanService` | Describe planned material quantity and price for a maintenance activity. | `/services/pulse/maintenance/plan/materials` |

### Resource usage

| Service | Description | Base path |
| --- | --- | --- |
| [**Maintenance energy source usage**](../integration/maintenance/maintenance-energy-source-usage.md)<br>`MaintenanceEnergySourceUsageService` | Record actual energy quantity and price for a maintenance activity. | `/services/pulse/maintenance/usage/energy-sources` |
| [**Maintenance equipment usage**](../integration/maintenance/maintenance-equipment-usage.md)<br>`MaintenanceEquipmentUsageService` | Record actual equipment hours and hourly price for a maintenance activity. | `/services/pulse/maintenance/usage/equipment` |
| [**Maintenance expense usage**](../integration/maintenance/maintenance-expense-usage.md)<br>`MaintenanceExpenseUsageService` | Record an additional cost incurred during a maintenance activity. | `/services/pulse/maintenance/usage/expenses` |
| [**Maintenance labor usage**](../integration/maintenance/maintenance-labor-usage.md)<br>`MaintenanceLaborUsageService` | Record actual labor hours and hourly price for a maintenance activity. | `/services/pulse/maintenance/usage/labor` |
| [**Maintenance material usage**](../integration/maintenance/maintenance-material-usage.md)<br>`MaintenanceMaterialUsageService` | Record actual material quantity and price for a maintenance activity. | `/services/pulse/maintenance/usage/materials` |

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