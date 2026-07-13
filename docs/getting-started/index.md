# Integration overview

The Pulse API receives operational data from customer systems and maps it to the Pulse data model.

Pulse uses this data to connect business activities, identify patterns, evaluate their impact, and generate recommendations.

Typical source systems include ERP, MES, SCADA, IoT, maintenance, quality, logistics, and supply-chain applications.

```text
Customer systems
→ Data mapping
→ Pulse API
→ Pulse data model
→ Analyses and recommendations
```

## Requirements

Before starting an integration, you need:

- access to the Pulse API,
- valid authentication credentials,
- a system or service that can send requests to the API,
- access to the relevant customer data,
- stable identifiers for records and related entities,
- agreed timestamp, time-zone, unit, and currency formats,
- and a mapping between customer data and Pulse entities.

The exact authentication method, supported operations, and request formats are described in the API documentation.

## Data provided to Pulse

An integration may send:

- organizational and operational structures,
- products, materials, equipment, customers, and suppliers,
- batches and their stages,
- planned and actual activity,
- labour, equipment, material, and energy usage,
- quantities and business results,
- downtime, delays, waste, and expenses,
- shifts and operating periods,
- lots and traceability records,
- and sensor or process measurements.

Only data that is available and relevant to the integration needs to be provided.

## Source systems

Production and operational data may be distributed across several systems.

| Source system | Typical data |
| --- | --- |
| ERP | Orders, products, materials, customers, suppliers, quantities, and planned activity |
| MES | Operations, execution records, labour, equipment usage, output, and downtime |
| SCADA | Machine states, alarms, measurements, and equipment events |
| IoT platform | Sensor and environmental measurements |
| Maintenance system | Maintenance activity, reasons, duration, and costs |
| Quality system | Inspections, defects, rejected quantities, and quality results |
| Logistics or supply system | Deliveries, movements, supply activity, and related operational records |

The integration must identify which system is the authoritative source for each type of data.

## Mapping customer data

Customer records must be mapped to the corresponding Pulse entities.

Examples include:

| Customer concept | Possible Pulse concept |
| --- | --- |
| Business process instance | Batch |
| Process step or operation | Stage |
| Production order | Batch used in a production context |
| Supply activity | Batch used in a supply context |
| Machine or asset | Equipment |
| Material issue or consumption | Material usage |
| Labour record | Labour usage |
| Machine stop | Downtime |
| Sensor reading | Measurement |
| Completed or recorded result | Produced or another applicable result entity |

A batch is not limited to production. It represents a unit of business activity and may be used in production, supply, logistics, or another operational context.

The final mapping depends on the customer process, the available source data, and the supported Pulse API operations.

## Integration responsibilities

The customer or integrator is responsible for:

- identifying the source systems,
- defining the data mapping,
- maintaining consistent identifiers,
- sending data in the required format,
- handling updates and corrections,
- and validating the submitted data.

Pulse is responsible for:

- authenticating and validating requests,
- storing submitted source data,
- connecting related entities,
- processing the available context,
- and generating analyses and recommendations.

## Implementation sequence

A typical integration follows this sequence:

1. Identify the customer processes and source systems.
2. Identify the data available in each system.
3. Map customer records to Pulse entities.
4. Synchronize the required reference data.
5. Send batches, stages, activities, measurements, and related records.
6. Validate identifiers and relationships.
7. Test one complete end-to-end scenario.
8. Configure ongoing synchronization.

## Next steps

- [Integration architecture](architecture.md)
- [Implementation roadmap](implementation-roadmap.md)
- [Pulse data model](../data-model/index.md)
- [A typical operational scenario](../scenarios/typical-production-day.md)
- [API overview](../api/index.md)