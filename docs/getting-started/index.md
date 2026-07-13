# Integration overview

The Pulse API receives operational data from customer systems and maps it to the Pulse data model.

Pulse uses this data to connect business activities, identify patterns, evaluate their impact, and generate recommendations.

```text
Customer systems
→ Data mapping
→ Pulse API
→ Pulse data model
→ Analyses and recommendations
```

Typical source systems include ERP, MES, SCADA, IoT, maintenance, quality, logistics, and supply-chain applications.

## Before you start

You need:

- access to the Pulse API,
- valid authentication credentials,
- a system or service that can send API requests,
- access to the relevant customer data,
- stable identifiers for related records,
- and an initial mapping between customer data and Pulse entities.

See:

- [Authenticate](authentication.md)
- [Map your data](../integration/index.md)
- [Pulse data model](../data-model/index.md)
- [API reference](../api/index.md)

## Prepare your data

Identify which customer systems contain the required operational data and which system is the authoritative source for each record.

Then map customer concepts—such as business activities, operations, equipment, material usage, downtime, and measurements—to the corresponding Pulse entities.

See [Integration](../integration/index.md) for details.

## Responsibilities

The customer or integrator is responsible for:

- identifying source systems,
- mapping customer records to Pulse,
- maintaining consistent identifiers,
- sending valid data,
- and handling updates and corrections.

Pulse is responsible for:

- authenticating and validating requests,
- storing submitted source data,
- connecting related entities,
- and generating analyses and recommendations.

## Integration workflow

A typical integration follows this sequence:

1. Identify the relevant source systems and data.
2. [Map customer records to Pulse](../integration/index.md).
3. [Synchronize the required master data](../integration/master-data.md).
4. [Send operational activity](../integration/operational-data.md).
5. [Send measurements](../integration/measurements.md).
6. [Validate the result](validation.md).
7. [Handle updates and errors](../integration/updates-and-errors.md).

## Next steps

- [Authenticate](authentication.md)
- [Send your first data](first-request.md)
- [Typical operational day](../scenarios/typical-operational-day.md)
- [Pulse data model](../data-model/index.md)
- [API reference](../api/index.md)