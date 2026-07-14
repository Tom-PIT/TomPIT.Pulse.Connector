# API reference

The PULSE API is a REST API for submitting operational data and retrieving PULSE results.

This page provides an index of the available API families and services. Use [Scalar](https://scalar.com/) for the complete operation-level reference, including request fields, parameters, schemas, and responses.

[Open the complete API reference in Scalar](#)

## Using this reference

Use this page to identify the relevant API family and service.

Open the service in Scalar to inspect:

- available operations,
- request fields and data types,
- query parameters,
- allowed values,
- response schemas,
- and example requests where available.

Use Scalar to inspect and test individual requests. Use integration code for continuous or high-volume data exchange.

## API families

<div class="grid cards" markdown>

-   [**Master data**](#master-data)
-   [**Manufacturing**](#manufacturing)
-   [**Maintenance**](#maintenance)
-   [**Calculations**](#calculations)
-   [**Signals**](#signals)
-   [**Analysis and recommendations**](#analysis-and-recommendations)
-   [**Configuration**](#configuration)
-   [**Frontend**](#frontend)

</div>

## API conventions

Most resources expose a consistent set of operations:

| Operation | HTTP method | Purpose |
| --- | --- | --- |
| `insert` | `POST` | Create a record |
| `update` | `PUT` | Update an existing record |
| `patch` | `PATCH` | Update selected properties |
| `delete` | `DELETE` | Delete a record |
| `select` | `GET` | Retrieve one record by its PULSE ID |
| `query` | `GET` | Retrieve and filter multiple records |

Insert operations commonly return the integer ID assigned to the new record. Store this ID and use it when submitting related records.

## Master data

Create and maintain the reference data used by operational records.

| Service | Description | Base path |
| --- | --- | --- |
| **Ambient Type**<br>`AmbientTypeService` | Define measurement types, units, and expected value ranges. | `/services/pulse/types/ambient-types` |
| **Ambient Type Service Extensions**<br>`AmbientTypeServiceExtensions` | Create and maintain ambient type service extensions master data. | `/services/pulse/types/ambient-types/update-batch` |
| **Customer**<br>`CustomerService` | Create and maintain customer master data. | `/services/pulse/types/customers` |
| **Customer Extensions**<br>`CustomerExtensions` | Create and maintain customer extensions master data. | `/services/pulse/types/customers/update-batch` |
| **Delay**<br>`DelayService` | Create and maintain delay master data. | `/services/pulse/types/delays` |
| **Delay Extensions**<br>`DelayExtensions` | Create and maintain delay extensions master data. | `/services/pulse/types/delays/update-batch` |
| **Downtime Category**<br>`DowntimeCategoryService` | Define downtime category used to classify downtime. | `/services/pulse/types/downtime-categories` |
| **Downtime Category Service Extensions**<br>`DowntimeCategoryServiceExtensions` | Define downtime category service extensions used to classify downtime. | `/services/pulse/types/downtime-categories/update-batch` |
| **Downtime Cause**<br>`DowntimeCauseService` | Define downtime cause used to classify downtime. | `/services/pulse/types/downtime-causes` |
| **Downtime Cause Service Extensions**<br>`DowntimeCauseServiceExtensions` | Define downtime cause service extensions used to classify downtime. | `/services/pulse/types/downtime-causes/update-batch` |
| **Downtime Type**<br>`DowntimeTypeService` | Define downtime type used to classify downtime. | `/services/pulse/types/downtime-types` |
| **Downtime Type Service Extensions**<br>`DowntimeTypeServiceExtensions` | Define downtime type service extensions used to classify downtime. | `/services/pulse/types/downtime-types/update-batch` |
| **Energy Source**<br>`EnergySourceService` | Create and maintain energy source master data. | `/services/pulse/types/energy-sources` |
| **Energy Source Extensions**<br>`EnergySourceExtensions` | Create and maintain energy source extensions master data. | `/services/pulse/types/energy-sources/update-batch` |
| **Equipment**<br>`EquipmentService` | Create and maintain equipment master data. | `/services/pulse/types/equipment` |
| **Equipment Extensions**<br>`EquipmentExtensions` | Create and maintain equipment extensions master data. | `/services/pulse/types/equipment/update-batch` |
| **Expense**<br>`ExpenseService` | Create and maintain expense master data. | `/services/pulse/types/expenses` |
| **Expense Extensions**<br>`ExpenseExtensions` | Create and maintain expense extensions master data. | `/services/pulse/types/expenses/update-batch` |
| **Labor**<br>`LaborService` | Create and maintain labor master data. | `/services/pulse/types/labor` |
| **Material**<br>`MaterialService` | Create and maintain material master data. | `/services/pulse/types/materials` |
| **Material Extensions**<br>`MaterialExtensions` | Create and maintain material extensions master data. | `/services/pulse/types/materials/update-batch` |
| **Measure Unit**<br>`MeasureUnitService` | Create and maintain measure unit master data. | `/services/pulse/types/measure-units` |
| **Measure Unit Service Extensions**<br>`MeasureUnitServiceExtensions` | Create and maintain measure unit service extensions master data. | `/services/pulse/types/measure-units/update-batch` |
| **Person Extensions**<br>`PersonExtensions` | Create and maintain person extensions master data. | `/services/pulse/types/labor/update-batch` |
| **Plant**<br>`PlantService` | Manage planned plant data. | `/services/pulse/types/plants` |
| **Plant Service Extensions**<br>`PlantServiceExtensions` | Manage planned plant service extensions data. | `/services/pulse/types/plants/update-batch` |
| **Product**<br>`ProductService` | Create and maintain product master data. | `/services/pulse/types/products` |
| **Product Extensions**<br>`ProductExtensions` | Create and maintain product extensions master data. | `/services/pulse/types/products/update-batch` |
| **Production Line**<br>`ProductionLineService` | Create and maintain production line master data. | `/services/pulse/types/production-lines` |
| **Production Line Extensions**<br>`ProductionLineExtensions` | Create and maintain production line extensions master data. | `/services/pulse/types/production-lines/update-batch` |
| **Shift**<br>`ShiftService` | Create and maintain shift master data. | `/services/pulse/types/shifts` |
| **Shift Service Extensions**<br>`ShiftServiceExtensions` | Create and maintain shift service extensions master data. | `/services/pulse/types/shifts/update-batch` |
| **Supplier**<br>`SupplierService` | Create and maintain supplier master data. | `/services/pulse/types/suppliers` |
| **Supplier Extensions**<br>`SupplierExtensions` | Create and maintain supplier extensions master data. | `/services/pulse/types/suppliers/update-batch` |
| **Waste Type**<br>`WasteTypeService` | Define waste type used to classify waste. | `/services/pulse/types/waste-types` |
| **Waste Type Service Extensions**<br>`WasteTypeServiceExtensions` | Define waste type service extensions used to classify waste. | `/services/pulse/types/waste-types/update-batch` |

## Manufacturing

Submit planned and actual manufacturing activity, resource usage, output, downtime, waste, and measurements.

| Service | Description | Base path |
| --- | --- | --- |
| **Ambient Value**<br>`AmbientValueService` | Submit and retrieve measured values linked to PULSE entities. | `/services/pulse/manufacturing/ambient-values` |
| **Ambient Value Extensions**<br>`AmbientValueExtensions` | Manage ambient value extensions records. | `/services/pulse/manufacturing/ambient-values/update-batch` |
| **Batch**<br>`BatchService` | Manage manufacturing batches and their core business context. | `/services/pulse/manufacturing/batches` |
| **Batch Plan**<br>`BatchPlanService` | Manage planned batch data. | `/services/pulse/manufacturing/batches/plan` |
| **Batch Service Extensions**<br>`BatchServiceExtensions` | Manage batch service extensions records. | `/services/pulse/manufacturing/batches/update-batch` |
| **Batch Shift**<br>`BatchShiftService` | Manage batch shift records. | `/services/pulse/manufacturing/batches/shifts` |
| **Batch Shift Extensions**<br>`BatchShiftExtensions` | Manage batch shift extensions records. | `/services/pulse/manufacturing/batches/shifts/update-batch` |
| **Batch Usage**<br>`BatchUsageService` | Record and retrieve actual batch usage. | `/services/pulse/manufacturing/batches/usage` |
| **Downtime**<br>`DowntimeService` | Record and retrieve downtime data. | `/services/pulse/manufacturing/batches/stages/downtime` |
| **Downtime Extensions**<br>`DowntimeExtensions` | Record and retrieve downtime extensions data. | `/services/pulse/manufacturing/batches/stages/downtime/update-batch` |
| **Downtime Maintenance**<br>`DowntimeMaintenanceService` | Record and retrieve downtime maintenance data. | `/services/pulse/manufacturing/batches/stages/downtime/maintenance` |
| **Downtime Plan**<br>`DowntimePlanService` | Manage planned downtime data. | `/services/pulse/manufacturing/batches/stages/downtime/plan` |
| **Downtime Usage**<br>`DowntimeUsageService` | Record and retrieve actual downtime usage. | `/services/pulse/manufacturing/batches/stages/downtime/usage` |
| **Energy Source Plan**<br>`EnergySourcePlanService` | Manage planned energy source data. | `/services/pulse/manufacturing/batches/stages/plan/energy-sources` |
| **Energy Source Plan Extensions**<br>`EnergySourcePlanExtensions` | Manage planned energy source extensions data. | `/services/pulse/manufacturing/batches/stages/plan/energy-sources/update-batch` |
| **Energy Source Usage**<br>`EnergySourceUsageService` | Record and retrieve actual energy source usage. | `/services/pulse/manufacturing/batches/stages/usage/energy-sources` |
| **Energy Source Usage Extensions**<br>`EnergySourceUsageExtensions` | Record and retrieve actual energy source extensions usage. | `/services/pulse/manufacturing/batches/stages/usage/energy-sources/update-batch` |
| **Equipment Plan**<br>`EquipmentPlanService` | Manage planned equipment data. | `/services/pulse/manufacturing/batches/stages/plan/equipment` |
| **Equipment Plan Extensions**<br>`EquipmentPlanExtensions` | Manage planned equipment extensions data. | `/services/pulse/manufacturing/batches/stages/plan/equipment/update-batch` |
| **Equipment Plan Period**<br>`EquipmentPlanPeriodService` | Manage time periods associated with equipment plan records. | `/services/pulse/manufacturing/batches/stages/plan/equipment/periods` |
| **Equipment Plan Period Extensions**<br>`EquipmentPlanPeriodExtensions` | Manage time periods associated with equipment plan extensions records. | `/services/pulse/manufacturing/batches/stages/plan/equipment/periods/update-batch` |
| **Equipment Usage**<br>`EquipmentUsageService` | Record and retrieve actual equipment usage. | `/services/pulse/manufacturing/batches/stages/usage/equipment` |
| **Equipment Usage Extensions**<br>`EquipmentUsageExtensions` | Record and retrieve actual equipment extensions usage. | `/services/pulse/manufacturing/batches/stages/usage/equipment/update-batch` |
| **Equipment Usage Period**<br>`EquipmentUsagePeriodService` | Manage time periods associated with equipment usage records. | `/services/pulse/manufacturing/batches/stages/usage/equipment/periods` |
| **Equipment Usage Period Extensions**<br>`EquipmentUsagePeriodExtensions` | Manage time periods associated with equipment usage extensions records. | `/services/pulse/manufacturing/batches/stages/usage/equipment/periods/update-batch` |
| **Expense Plan**<br>`ExpensePlanService` | Manage planned expense data. | `/services/pulse/manufacturing/batches/stages/plan/expenses` |
| **Expense Plan Extensions**<br>`ExpensePlanExtensions` | Manage planned expense extensions data. | `/services/pulse/manufacturing/batches/stages/plan/expenses/update-batch` |
| **Expense Usage**<br>`ExpenseUsageService` | Record and retrieve actual expense usage. | `/services/pulse/manufacturing/batches/stages/usage/expenses` |
| **Expense Usage Extensions**<br>`ExpenseUsageExtensions` | Record and retrieve actual expense extensions usage. | `/services/pulse/manufacturing/batches/stages/usage/expenses/update-batch` |
| **Labor Plan**<br>`LaborPlanService` | Manage planned labor data. | `/services/pulse/manufacturing/batches/stages/plan/labor` |
| **Labor Plan Extensions**<br>`LaborPlanExtensions` | Manage planned labor extensions data. | `/services/pulse/manufacturing/batches/stages/plan/labor/update-batch` |
| **Labor Usage**<br>`LaborUsageService` | Record and retrieve actual labor usage. | `/services/pulse/manufacturing/batches/stages/usage/labor` |
| **Labor Usage Extensions**<br>`LaborUsageExtensions` | Record and retrieve actual labor extensions usage. | `/services/pulse/manufacturing/batches/stages/usage/labor/update-batch` |
| **Labor Usage Period**<br>`LaborUsagePeriodService` | Manage time periods associated with labor usage records. | `/services/pulse/manufacturing/batches/stages/usage/labor/periods` |
| **Labor Usage Period Extensions**<br>`LaborUsagePeriodExtensions` | Manage time periods associated with labor usage extensions records. | `/services/pulse/manufacturing/batches/stages/usage/labor/periods/update-batch` |
| **Material Plan**<br>`MaterialPlanService` | Manage planned material data. | `/services/pulse/manufacturing/batches/stages/plan/materials` |
| **Material Plan Extensions**<br>`MaterialPlanExtensions` | Manage planned material extensions data. | `/services/pulse/manufacturing/batches/stages/plan/materials/update-batch` |
| **Material Usage**<br>`MaterialUsageService` | Record and retrieve actual material usage. | `/services/pulse/manufacturing/batches/stages/usage/materials` |
| **Material Usage Extensions**<br>`MaterialUsageExtensions` | Record and retrieve actual material extensions usage. | `/services/pulse/manufacturing/batches/stages/usage/materials/update-batch` |
| **Produced**<br>`ProducedService` | Record produced quantities and their quality classification. | `/services/pulse/manufacturing/batches/produced` |
| **Produced Extensions**<br>`ProducedExtensions` | Manage produced extensions records. | `/services/pulse/manufacturing/batches/produced/update-batch` |
| **Stage**<br>`StageService` | Manage stages belonging to manufacturing batches. | `/services/pulse/manufacturing/batches/stages` |
| **Stage Delay**<br>`StageDelayService` | Manage stage delay records. | `/services/pulse/manufacturing/batches/stages/delays` |
| **Stage Delay Extensions**<br>`StageDelayExtensions` | Manage stage delay extensions records. | `/services/pulse/manufacturing/batches/stages/delays/update-batch` |
| **Stage Extensions**<br>`StageExtensions` | Manage stage extensions records. | `/services/pulse/manufacturing/batches/stages/update-batch` |
| **Stage Plan**<br>`StagePlanService` | Manage planned stage data. | `/services/pulse/manufacturing/batches/stages/plan` |
| **Stage Usage**<br>`StageUsageService` | Record and retrieve actual stage usage. | `/services/pulse/manufacturing/batches/stages/usage` |
| **Waste**<br>`WasteService` | Record and retrieve waste data. | `/services/pulse/manufacturing/batches/stages/usage/waste` |
| **Waste Energy Source Usage**<br>`WasteEnergySourceUsageService` | Record and retrieve actual waste energy source usage. | `/services/pulse/manufacturing/batches/stages/usage/waste/energy-sources` |
| **Waste Energy Source Usage Extensions**<br>`WasteEnergySourceUsageExtensions` | Record and retrieve actual waste energy source extensions usage. | `/services/pulse/manufacturing/batches/stages/usage/waste/energy-sources/update-batch` |
| **Waste Expense Usage**<br>`WasteExpenseUsageService` | Record and retrieve actual waste expense usage. | `/services/pulse/manufacturing/batches/stages/usage/waste/expenses` |
| **Waste Expense Usage Extensions**<br>`WasteExpenseUsageExtensions` | Record and retrieve actual waste expense extensions usage. | `/services/pulse/manufacturing/batches/stages/usage/waste/expenses/update-batch` |
| **Waste Extensions**<br>`WasteExtensions` | Record and retrieve waste extensions data. | `/services/pulse/manufacturing/batches/stages/usage/waste/update-batch` |
| **Waste Material Usage**<br>`WasteMaterialUsageService` | Record and retrieve actual waste material usage. | `/services/pulse/manufacturing/batches/stages/usage/waste/materials` |
| **Waste Material Usage Extensions**<br>`WasteMaterialUsageExtensions` | Record and retrieve actual waste material extensions usage. | `/services/pulse/manufacturing/batches/stages/usage/waste/materials/update-batch` |

## Maintenance

Submit maintenance plans, activity, resource usage, downtime, and related records.

| Service | Description | Base path |
| --- | --- | --- |
| **Labor Plan Period**<br>`LaborPlanPeriodService` | Manage time periods associated with labor plan records. | `/services/pulse/maintenance/plan/labor/periods` |
| **Labor Plan Period Extensions**<br>`LaborPlanPeriodExtensions` | Manage time periods associated with labor plan extensions records. | `/services/pulse/maintenance/plan/labor/periods/update-batch` |
| **Maintenance**<br>`MaintenanceService` | Manage maintenance records for maintenance activities. | `/services/pulse/maintenance` |
| **Maintenance Energy Source Plan**<br>`MaintenanceEnergySourcePlanService` | Manage planned maintenance energy source data. | `/services/pulse/maintenance/estimations/energy-sources` |
| **Maintenance Energy Source Plan Extensions**<br>`MaintenanceEnergySourcePlanExtensions` | Manage planned maintenance energy source extensions data. | `/services/pulse/maintenance/estimations/energy-sources/update-batch` |
| **Maintenance Energy Source Usage**<br>`MaintenanceEnergySourceUsageService` | Record and retrieve actual maintenance energy source usage. | `/services/pulse/maintenance/usage/energy-sources` |
| **Maintenance Energy Source Usage Extensions**<br>`MaintenanceEnergySourceUsageExtensions` | Record and retrieve actual maintenance energy source extensions usage. | `/services/pulse/maintenance/usage/energy-sources/update-batch` |
| **Maintenance Equipment Plan**<br>`MaintenanceEquipmentPlanService` | Manage planned maintenance equipment data. | `/services/pulse/maintenance/plan/equipment` |
| **Maintenance Equipment Plan Extensions**<br>`MaintenanceEquipmentPlanExtensions` | Manage planned maintenance equipment extensions data. | `/services/pulse/maintenance/plan/equipment/update-batch` |
| **Maintenance Equipment Usage**<br>`MaintenanceEquipmentUsageService` | Record and retrieve actual maintenance equipment usage. | `/services/pulse/maintenance/usage/equipment` |
| **Maintenance Equipment Usage Extensions**<br>`MaintenanceEquipmentUsageExtensions` | Record and retrieve actual maintenance equipment extensions usage. | `/services/pulse/maintenance/usage/equipment/update-batch` |
| **Maintenance Expense Plan**<br>`MaintenanceExpensePlanService` | Manage planned maintenance expense data. | `/services/pulse/maintenance/plan/expenses` |
| **Maintenance Expense Plan Extensions**<br>`MaintenanceExpensePlanExtensions` | Manage planned maintenance expense extensions data. | `/services/pulse/maintenance/plan/expenses/update-batch` |
| **Maintenance Expense Usage**<br>`MaintenanceExpenseUsageService` | Record and retrieve actual maintenance expense usage. | `/services/pulse/maintenance/usage/expenses` |
| **Maintenance Expense Usage Extensions**<br>`MaintenanceExpenseUsageExtensions` | Record and retrieve actual maintenance expense extensions usage. | `/services/pulse/maintenance/usage/expenses/update-batch` |
| **Maintenance Extensions**<br>`MaintenanceExtensions` | Manage maintenance extensions records for maintenance activities. | `/services/pulse/maintenance/update-batch` |
| **Maintenance Labor Plan**<br>`MaintenanceLaborPlanService` | Manage planned maintenance labor data. | `/services/pulse/maintenance/plan/labor` |
| **Maintenance Labor Plan Extensions**<br>`MaintenanceLaborPlanExtensions` | Manage planned maintenance labor extensions data. | `/services/pulse/maintenance/plan/labor/update-batch` |
| **Maintenance Labor Usage**<br>`MaintenanceLaborUsageService` | Record and retrieve actual maintenance labor usage. | `/services/pulse/maintenance/usage/labor` |
| **Maintenance Labor Usage Extensions**<br>`MaintenanceLaborUsageExtensions` | Record and retrieve actual maintenance labor extensions usage. | `/services/pulse/maintenance/usage/labor/update-batch` |
| **Maintenance Material Plan**<br>`MaintenanceMaterialPlanService` | Manage planned maintenance material data. | `/services/pulse/maintenance/plan/materials` |
| **Maintenance Material Plan Extensions**<br>`MaintenanceMaterialPlanExtensions` | Manage planned maintenance material extensions data. | `/services/pulse/maintenance/plan/materials/update-batch` |
| **Maintenance Material Usage**<br>`MaintenanceMaterialUsageService` | Record and retrieve actual maintenance material usage. | `/services/pulse/maintenance/usage/materials` |
| **Maintenance Material Usage Extensions**<br>`MaintenanceMaterialUsageExtensions` | Record and retrieve actual maintenance material extensions usage. | `/services/pulse/maintenance/usage/materials/update-batch` |
| **Maintenance Plan**<br>`MaintenancePlanService` | Manage planned maintenance data. | `/services/pulse/maintenance/plan` |
| **Maintenance Usage**<br>`MaintenanceUsageService` | Record and retrieve actual maintenance usage. | `/services/pulse/maintenance/usage` |

## Calculations

Run calculations and retrieve calculated PULSE metrics.

| Service | Description | Base path |
| --- | --- | --- |
| **Batch Performance Index**<br>`BatchPerformanceIndexService` | Run or retrieve batch performance index calculations. | `/services/pulse/calculations/manufacturing/batches/performance-index` |
| **Batch Plan Calculation**<br>`BatchPlanCalculationService` | Manage planned batch calculation data. | `/services/pulse/calculations/manufacturing/batches/plan` |
| **Batch Usage Calculation**<br>`BatchUsageCalculationService` | Record and retrieve actual batch calculation usage. | `/services/pulse/calculations/manufacturing/batches/usage` |
| **Downtime Plan Calculation**<br>`DowntimePlanCalculationService` | Manage planned downtime calculation data. | `/services/pulse/calculations/manufacturing/downtime/plan` |
| **Downtime Usage Calculation**<br>`DowntimeUsageCalculationService` | Record and retrieve actual downtime calculation usage. | `/services/pulse/calculations/manufacturing/downtime/usage` |
| **Maintenance Plan Calculation**<br>`MaintenancePlanCalculationService` | Manage planned maintenance calculation data. | `/services/pulse/calculations/maintenance/plan` |
| **Maintenance Usage Calculation**<br>`MaintenanceUsageCalculationService` | Record and retrieve actual maintenance calculation usage. | `/services/pulse/calculations/maintenance/usage` |
| **Stage Plan Calculation**<br>`StagePlanCalculationService` | Manage planned stage calculation data. | `/services/pulse/calculations/manufacturing/stages/plan` |

## Signals

Retrieve signals generated from submitted operational data.

| Service | Description | Base path |
| --- | --- | --- |
| **Profitability Signal**<br>`ProfitabilitySignalService` | Retrieve profitability signal signals generated by PULSE. | `/services/pulse/signals/manufacturing/profitability` |
| **Signal Dimension**<br>`SignalDimensionService` | Retrieve signal dimension signals generated by PULSE. | `/services/pulse/signals/manufacturing/signal-dimensions` |

## Analysis and recommendations

Retrieve analytical results, insights, recommendations, forecasts, and improvement actions.

| Service | Description | Base path |
| --- | --- | --- |
| **Assessment**<br>`AssessmentService` | Retrieve assessment produced by PULSE analysis. | `/services/pulse/analysis/batches/assessment` |
| **Assessment Content**<br>`AssessmentContentService` | Retrieve assessment content produced by PULSE analysis. | `/services/pulse/analysis/batches/assessment/content` |
| **Conclusion**<br>`ConclusionService` | Retrieve conclusion produced by PULSE analysis. | `/services/pulse/analysis/batches/assessment/measurements` |
| **Conclusion Content**<br>`ConclusionContentService` | Retrieve conclusion content produced by PULSE analysis. | `/services/pulse/analysis/batches/assessment/measurements/content` |
| **Forecast**<br>`ForecastService` | Retrieve forecast produced by PULSE analysis. | `/services/pulse/analysis/forecasts` |
| **Forecast Content**<br>`ForecastContentService` | Retrieve forecast content produced by PULSE analysis. | `/services/pulse/analysis/forecasts/content` |
| **Improvement Action**<br>`ImprovementActionService` | Retrieve improvement action produced by PULSE analysis. | `/services/pulse/analysis/improvements/actions` |
| **Improvement Action Content**<br>`ImprovementActionContentService` | Retrieve improvement action content produced by PULSE analysis. | `/services/pulse/analysis/improvements/actions/content` |
| **Insight**<br>`InsightService` | Retrieve insight produced by PULSE analysis. | `/services/pulse/analysis/improvements/insights` |
| **Insight Content**<br>`InsightContentService` | Retrieve insight content produced by PULSE analysis. | `/services/pulse/analysis/improvements/insights/content` |
| **Recommendation**<br>`RecommendationService` | Retrieve recommendation produced by PULSE analysis. | `/services/pulse/analysis/improvements/recommendations` |
| **Recommendation Content**<br>`RecommendationContentService` | Retrieve recommendation content produced by PULSE analysis. | `/services/pulse/analysis/improvements/recommendations/content` |
| **Recommendation Scope**<br>`RecommendationScopeService` | Retrieve recommendation scope produced by PULSE analysis. | `/services/pulse/analysis/improvements/scopes` |

## Configuration

Manage API and PULSE configuration exposed by the specification.

| Service | Description | Base path |
| --- | --- | --- |
| **Configuration**<br>`ConfigurationService` | Manage configuration settings. | `/services/pulse/configuration` |

## Frontend

Retrieve supporting data used by PULSE user interfaces.

| Service | Description | Base path |
| --- | --- | --- |
| **Analysis**<br>`AnalysisService` | Retrieve analysis data used by PULSE interfaces. | `/services/pulse/frontend/analysis` |
| **Navigation**<br>`NavigationService` | Retrieve navigation data used by PULSE interfaces. | `/services/pulse/frontend/navigation` |
| **Reason**<br>`ReasonService` | Retrieve reason data used by PULSE interfaces. | `/services/pulse/frontend/why` |