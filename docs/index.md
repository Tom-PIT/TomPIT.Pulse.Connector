# Pulse Integration Documentation

Pulse is a production-intelligence platform that uses operational data to identify patterns, evaluate their business impact, and generate actionable recommendations.

This documentation explains how to integrate customer systems with Pulse. It connects real production processes—such as production orders, machine activity, labour, material consumption, downtime, waste, and sensor measurements—to the Pulse data model and API.

## How the integration works

Customer production data may originate from systems such as ERP, MES, SCADA, IoT platforms, or other operational applications.

The integration transforms this data into the structure expected by Pulse:

```text
Customer production process
→ Source systems
→ Data mapping
→ Pulse API
→ Pulse data model
→ Analysis and recommendations
```

The quality and completeness of the source data directly affect the value Pulse can produce.

## What this documentation covers

This documentation will help you:

* understand the Pulse data model,
* identify the production data required by Pulse,
* map customer-system records to Pulse entities,
* send master data and production data through the API,
* connect sensor measurements to production context,
* synchronize updates and corrections,
* validate data quality,
* and test an integration before go-live.

## Who this documentation is for

This documentation is intended for:

* customer integration teams,
* ERP and MES integrators,
* SCADA and IoT integrators,
* software developers,
* solution architects,
* and implementation consultants.

## Start here

Begin with the following sections:

* [Integration overview](getting-started/index.md)
* [Integration architecture](getting-started/architecture.md)
* [Pulse data model](data-model/index.md)
* [A typical production day](scenarios/typical-production-day.md)
* [API overview](api/index.md)
