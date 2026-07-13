# Integration overview

PULSE uses operational and production data to identify patterns, evaluate their business impact, and generate recommendations.

To provide this context, PULSE integrates with the systems that manage or observe the customer’s production processes. These may include ERP, MES, SCADA, IoT, maintenance, quality, and other operational systems.

The integration transforms data from these source systems into the PULSE data model and sends it through the PULSE API.

```text
Customer production process
→ Source systems
→ Data mapping
→ PULSE API
→ PULSE data model
→ Analysis and recommendations
```

## What the integration provides

A PULSE integration connects real production activity with the entities and relationships used by PULSE.

Depending on the available source data, the integration may provide information about:

production orders and operations,
plants, production lines, and equipment,
products, materials, and lots,
planned and actual production times,
produced and rejected quantities,
labour and equipment usage,
material and energy consumption,
downtime and delays,
waste and additional expenses,
shifts,
and sensor or process measurements.

The completeness and quality of this information determine how much production context PULSE can analyse.

## Source systems

Production data is often distributed across several systems.

Typical sources include:

| Source system      | Typical information       |
| --- | --- |
| ERP                | Production orders, products, materials, customers, suppliers, planned quantities |
| MES                | Operations, production activity, labour, machine usage, output, downtime         |
| SCADA              | Machine states, alarms, process values, and equipment events                     |
| IoT platform       | Sensor measurements and environmental values                                     |
| Maintenance system | Equipment maintenance, reasons, duration, and costs                              |
| Quality system     | Inspections, rejected quantities, defects, and quality results                   |

A customer does not necessarily need to use all of these systems. The integration must identify which system owns each relevant type of data.

## Source data and PULSE data

The customer’s source systems remain responsible for the operational records they create.

The integration maps those records to the corresponding PULSE entities.

For example:

| Customer concept     | Possible PULSE concept |
| -------------------- | ---------------------- |
| Production order     | Batch                  |
| Production operation | Stage                  |
| Machine              | Equipment              |
| Material issue       | Material usage         |
| Operator time        | Labour usage           |
| Machine stop         | Downtime               |
| Sensor reading       | Measurement            |
| Completed quantity   | Produced quantity      |

These mappings are examples and must be confirmed for each customer and against the available PULSE API.

Integration responsibilities

A successful integration requires both technical connectivity and correct production context.

The customer or integrator is generally responsible for:

identifying the relevant source systems,
mapping source records to PULSE entities,
maintaining consistent identifiers,
sending records in the required structure,
handling updates and corrections,
and validating the completeness of the transferred data.

PULSE is responsible for:

receiving and validating the submitted data,
storing the source information in its data model,
connecting related production records,
calculating analyses and signals,
and producing recommendations from the available context.

The exact responsibilities and API behaviour will be documented as the integration interface is finalized.

Integration approach

A typical implementation will follow these stages:

Identify the customer’s source systems and available production data.
Map source-system concepts and identifiers to the PULSE data model.
Synchronize the required master data.
Send production plans and execution records.
Send measurements, downtime, waste, and other production events.
Validate the transferred data and entity relationships.
Test a complete production scenario before go-live.

## Next steps

Continue with:

- [Integration architecture](architecture.md)
- [Implementation roadmap](implementation-roadmap.md)
- [PULSE data model](../data-model/index.md)
- [A typical production day](../scenarios/typical-production-day.md)