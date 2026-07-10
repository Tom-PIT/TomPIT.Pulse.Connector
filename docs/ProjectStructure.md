# Proposed documentation structure

This is a proposed structure for the PULSE integration documentation. It is intended to be comprehensive, but the first public version may contain only a subset of the sections.

For information about the project’s purpose and target audience, see [README_INTERNAL.md](README_INTERNAL.md).

## 1. Introduction

The introduction should explain what PULSE is, what an integration provides, and how to use the documentation.

Possible pages:

```text
Introduction
├── What is PULSE?
├── What does an integration provide?
├── Who is this documentation for?
├── Integration responsibilities
└── Documentation conventions
```

### Main topics

* PULSE as a production-intelligence platform
* the role of customer data
* supported types of source systems
* responsibilities of PULSE and the customer
* documentation terminology and conventions

## 2. Integration overview

This section should provide a high-level understanding of how information moves from customer systems into PULSE.

Possible pages:

```text
Integration overview
├── How data flows into PULSE
├── Integration architecture
├── Source systems
├── Raw data and calculated data
├── Supported integration patterns
└── Recommended implementation sequence
```

### Main topics

* customer systems as owners of source data,
* PULSE as the owner of calculations and analyses,
* communication through the PULSE backend API,
* initial synchronization and ongoing synchronization,
* real-time and scheduled integration patterns,
* separation between raw records and calculated results.

A typical high-level flow may be represented as:

```text
ERP / MES / SCADA / IoT
→ Integration service
→ PULSE REST API
→ PULSE raw data layer
→ PULSE analyses and recommendations
```

## 3. Understanding the PULSE data model

This section should explain the main PULSE entities and their relationships independently of the API.

Possible pages:

```text
PULSE data model
├── Data-model overview
├── Entity relationships
├── Master data
├── Production structure
├── Planned and actual data
├── Measurements
├── Traceability
└── Entity identifiers
```

### Main topics

* plants,
* production lines,
* equipment,
* products,
* materials,
* batches,
* stages,
* production plans,
* actual usage,
* produced quantities,
* downtime,
* delays,
* waste,
* lots,
* measurements.

The central production structure currently appears to be:

```text
Plant
└── Production line
    └── Batch
        ├── Stage
        ├── Plan
        ├── Actual execution
        ├── Produced quantity
        ├── Shift
        ├── Waste
        └── Production-related usage
```

The documentation should also explain the likely mapping between common customer terminology and PULSE terminology.

For example:

```text
Customer work order
→ PULSE batch

Customer work-order operation
→ PULSE stage
```

This mapping must be confirmed against the API and domain knowledge.

## 4. Preparing the integration

This section should guide the customer through the analysis work that must happen before implementation begins.

Possible pages:

```text
Preparing the integration
├── Identify source systems
├── Identify available data
├── Define system ownership
├── Define identifiers
├── Define units and currencies
├── Define timestamps and time zones
├── Define synchronization frequency
└── Integration readiness checklist
```

### Main topics

* which source system owns each record,
* whether data originates in ERP, MES, SCADA, IoT, or another system,
* which source identifiers will be retained,
* how entities can be related across multiple systems,
* which units are used,
* which time zone is used,
* how often each type of data is available,
* whether historical data will be imported.

## 5. Master-data integration

This section should describe relatively stable reference data that must generally exist before production transactions are sent.

Possible pages:

```text
Master-data integration
├── Plants
├── Production lines
├── Equipment
├── Products
├── Materials
├── Measure units
├── Labour
├── Shifts
├── Customers
├── Suppliers
├── Downtime classifications
└── Waste classifications
```

### Main topics

For every master-data entity, the documentation should explain:

* what the entity represents,
* where it usually originates,
* how it is identified,
* which other entities reference it,
* when it should be synchronized,
* how changes should be handled,
* whether inactive records should remain available.

The master-data dependency structure should eventually be documented clearly.

For example:

```text
Plant
→ Production line
→ Equipment

Product
→ Batch

Material
→ Material plan / Material usage

Supplier
→ Lot
→ Material usage
```

## 6. Production integration

This section should explain how planned and actual production activity is represented.

Possible pages:

```text
Production integration
├── Production batches
├── Production stages
├── Batch plans
├── Stage plans
├── Batch execution
├── Stage execution
├── Produced quantities
├── Material consumption
├── Equipment usage
├── Labour usage
├── Energy usage
├── Additional expenses
├── Downtime
├── Delays
├── Waste
└── Shift allocation
```

### Main topics

* creating production context,
* planned versus actual dates,
* planned versus actual quantities,
* planned versus actual resource usage,
* work-order lifecycle,
* operation lifecycle,
* partial production reporting,
* order completion,
* corrections after completion.

The planned and actual model should be presented as a core PULSE concept.

For example:

```text
Batch
├── Batch plan
└── Batch usage

Stage
├── Stage plan
└── Stage usage
```

The same pattern may apply to:

```text
Material plan
↔ Material usage

Equipment plan
↔ Equipment usage

Labour plan
↔ Labour usage

Energy-source plan
↔ Energy-source usage

Expense plan
↔ Expense usage
```

## 7. Sensor and measurement integration

This section should describe how PULSE receives sensor, machine, process, and environmental measurements.

Possible pages:

```text
Sensor and measurement integration
├── Measurement model
├── Measurement types
├── Dimensions and entity references
├── Connecting measurements to equipment
├── Connecting measurements to production
├── Sampling frequency
├── Aggregated and raw measurements
├── Limits and expected values
└── Sensor-data examples
```

### Main topics

* measurement timestamp,
* measurement value,
* measurement type,
* measurement unit,
* minimum and maximum values,
* expected values,
* relationship to equipment or production,
* raw readings versus aggregated readings,
* acceptable sampling intervals,
* high-frequency data.

The relationship between a measurement and production context must be clarified.

Important questions include:

* which PULSE entities may be used as measurement dimensions,
* how the measurement references the related entity,
* whether measurements are associated directly with a batch or stage,
* whether PULSE derives production context using equipment and timestamps.

## 8. Traceability integration

This section should explain how materials, lots, suppliers, batches, and customers are connected.

Possible pages:

```text
Traceability integration
├── Lots
├── Supplier lots
├── Material-lot consumption
├── Customer traceability
├── Linking lots to batches
└── Traceability example
```

### Main topics

* supplier lot identifiers,
* internal lot identifiers,
* material consumption by lot,
* supplier-to-production traceability,
* production-to-customer traceability,
* tracing quality or waste patterns back to source lots.

A useful relationship example is:

```text
Supplier
→ Lot
→ Material usage
→ Production stage
→ Batch
→ Production result
→ Customer
```

## 9. End-to-end integration scenarios

This should be one of the most important and accessible sections of the documentation.

Instead of explaining entities only in isolation, it should show how a real production process is translated into PULSE data.

Possible pages:

```text
Integration scenarios
├── A typical production day
├── From work order to completed production
├── Recording labour and machine time
├── Capturing a machine stoppage
├── Recording waste and rejected output
├── Sending sensor measurements
├── Correcting production data
└── Multi-system integration example
```

### Typical production-day scenario

The primary scenario may follow this sequence:

```text
1. A production order is created.
2. The order is scheduled.
3. Production operations are created.
4. An operator starts an operation.
5. A machine begins operating.
6. Materials are consumed.
7. Sensors send measurements.
8. A machine stops unexpectedly.
9. Production resumes.
10. Good and rejected quantities are reported.
11. The order is completed.
12. PULSE evaluates the complete production context.
```

Each step should explain four perspectives:

| Perspective   | Description                      |
| ------------- | -------------------------------- |
| Production    | What happened in the factory     |
| Source system | Which system recorded the event  |
| PULSE model   | Which PULSE entity represents it |
| API           | Which API operation sends it     |

Example:

| Production event       | Source system   | PULSE entity           | API          |
| ---------------------- | --------------- | ---------------------- | ------------ |
| Work order created     | ERP             | Batch                  | To be linked |
| Operation created      | ERP or MES      | Stage                  | To be linked |
| Machine started        | MES or SCADA    | Equipment usage period | To be linked |
| Operator recorded work | MES             | Labour usage period    | To be linked |
| Temperature captured   | SCADA or sensor | Measurement            | To be linked |
| Machine stopped        | MES or SCADA    | Downtime               | To be linked |
| Material consumed      | ERP or MES      | Material usage         | To be linked |
| Quantity completed     | ERP or MES      | Produced               | To be linked |

This scenario should become the narrative backbone of the documentation.

## 10. Mapping customer data to PULSE

This section should explain how to analyse a customer’s source systems and create a formal mapping.

Possible pages:

```text
Data mapping
├── Mapping methodology
├── ERP-to-PULSE mapping
├── MES-to-PULSE mapping
├── SCADA-to-PULSE mapping
├── IoT-to-PULSE mapping
├── Typical source-field mappings
├── Mapping template
└── Mapping examples
```

### Main topics

* identifying source entities,
* identifying source fields,
* defining PULSE target entities,
* transformation rules,
* identifier mappings,
* unit conversion,
* status conversion,
* enumeration mapping,
* missing source data,
* default values,
* derived values.

Example mapping:

| Customer concept     | Typical source               | PULSE concept              |
| -------------------- | ---------------------------- | -------------------------- |
| Work order           | ERP production order         | Batch                      |
| Work-order operation | ERP routing or MES operation | Stage                      |
| Machine              | Asset register or MES        | Equipment                  |
| Operator time        | MES terminal                 | Labour usage               |
| Machine operation    | MES or SCADA                 | Equipment usage            |
| Sensor reading       | SCADA or IoT platform        | Measurement                |
| Unplanned stop       | MES or SCADA                 | Downtime                   |
| Scrap quantity       | ERP or MES                   | Waste or produced quantity |
| Material issue       | ERP or MES                   | Material usage             |

A reusable mapping worksheet should be included in this section.

## 11. API usage

This section should connect the conceptual model and integration workflows to the actual API.

Possible pages:

```text
API
├── API overview
├── Authentication
├── Base URLs and environments
├── Sending master data
├── Sending transactional data
├── Reading analyses
├── Request and response conventions
├── Pagination
├── Validation
├── Error responses
├── Retries
├── Idempotency
└── API reference
```

### Main topics

* Bearer-token authentication,
* available environments,
* content types,
* request structure,
* response structure,
* HTTP status codes,
* validation errors,
* retry behaviour,
* idempotent requests,
* batch requests,
* rate limits,
* reading PULSE results.

The API reference may link to generated Swagger documentation, but the integration guide should provide contextual examples around the endpoints.

## 12. Data synchronization

This section should explain how data is sent initially and kept synchronized over time.

Possible pages:

```text
Data synchronization
├── Initial data load
├── Incremental synchronization
├── Real-time integration
├── Scheduled integration
├── Record ordering
├── Updates and corrections
├── Duplicate prevention
├── Deleted and inactive records
└── Recovery after interruption
```

### Main topics

* synchronization order,
* dependency handling,
* historical imports,
* changed-record detection,
* correction of previously sent records,
* repeated requests,
* delayed data,
* recovery after network or system failure,
* inactive reference records,
* synchronization checkpoints.

A recommended sequence may eventually look like:

```text
1. Synchronize reference data.
2. Synchronize production structures.
3. Synchronize production plans.
4. Synchronize actual production activity.
5. Synchronize measurements and events.
6. Validate relationships and completeness.
```

The exact sequence must be confirmed against the API.

## 13. Data quality

This section should explain why complete and consistent data is required for meaningful PULSE results.

Possible pages:

```text
Data quality
├── Why data quality matters
├── Required production context
├── Identifier consistency
├── Timestamp quality
├── Unit consistency
├── Missing data
├── Overlapping periods
├── Impossible values
├── Validation checklist
└── Recommended quality indicators
```

### Main topics

* missing relationships,
* inconsistent identifiers,
* missing production context,
* invalid timestamps,
* incorrect time zones,
* duplicated records,
* overlapping periods,
* inconsistent units,
* impossible quantities,
* incomplete downtime records,
* missing production output.

The documentation should distinguish between:

* technically valid data,
* structurally complete data,
* and data that is sufficiently meaningful for analysis.

## 14. Testing and validation

This section should explain how an integration is verified before go-live.

Possible pages:

```text
Testing and validation
├── Integration-test strategy
├── Test environment
├── Minimum test dataset
├── Master-data validation
├── Production-flow validation
├── Sensor-data validation
├── Correction testing
├── Failure and retry testing
└── Acceptance checklist
```

### Main topics

* minimum viable dataset,
* test production order,
* test master data,
* complete production lifecycle,
* downtime test,
* waste test,
* sensor-measurement test,
* correction test,
* retry and duplicate test,
* final acceptance.

## 15. Troubleshooting

This section should provide practical guidance for common implementation problems.

Possible pages:

```text
Troubleshooting
├── Authentication errors
├── Validation errors
├── Missing references
├── Duplicate records
├── Incorrect units
├── Incorrect timestamps
├── Incomplete production context
├── Sensor-data issues
└── Support information
```

The troubleshooting pages should include:

* symptoms,
* likely causes,
* resolution steps,
* related API errors,
* and links to the relevant conceptual documentation.

## 16. Reference

The reference section should contain detailed material that is useful during implementation but does not need to interrupt the main learning flow.

Possible pages:

```text
Reference
├── Glossary
├── Entity reference
├── Relationship reference
├── Field reference
├── Enumerations
├── API reference
├── Example payloads
├── Mapping worksheets
└── Changelog
```

# Proposed initial public navigation

The full structure is intended as a long-term content plan. The initial public navigation should remain smaller and easier to understand.

```yaml
nav:
  - Home: index.md

  - Getting started:
      - Integration overview: getting-started/overview.md
      - Architecture: getting-started/architecture.md
      - Integration responsibilities: getting-started/responsibilities.md
      - Implementation roadmap: getting-started/implementation-roadmap.md

  - PULSE data model:
      - Overview: data-model/index.md
      - Entity relationships: data-model/entity-relationships.md
      - Master data: data-model/master-data.md
      - Production model: data-model/production.md
      - Planned and actual data: data-model/plan-and-actual.md
      - Measurements: data-model/measurements.md
      - Traceability: data-model/traceability.md

  - Integration guide:
      - Prepare the integration: integration/preparation.md
      - Master data: integration/master-data.md
      - Production data: integration/production.md
      - Sensor data: integration/sensor-data.md
      - Traceability data: integration/traceability.md
      - Data synchronization: integration/synchronization.md

  - Integration scenarios:
      - Typical production day: scenarios/typical-production-day.md
      - Work order lifecycle: scenarios/work-order-lifecycle.md
      - Machine downtime: scenarios/machine-downtime.md
      - Sensor capture: scenarios/sensor-capture.md
      - Waste and quality: scenarios/waste-and-quality.md

  - Mapping:
      - Mapping customer systems: mapping/index.md
      - ERP mapping: mapping/erp.md
      - MES mapping: mapping/mes.md
      - SCADA and IoT mapping: mapping/scada-iot.md
      - Mapping template: mapping/template.md

  - API:
      - API overview: api/index.md
      - Authentication: api/authentication.md
      - Requests and responses: api/conventions.md
      - Error handling: api/errors.md
      - API reference: api/reference.md

  - Implementation:
      - Data quality: implementation/data-quality.md
      - Testing: implementation/testing.md
      - Troubleshooting: implementation/troubleshooting.md
      - Go-live checklist: implementation/go-live-checklist.md

  - Reference:
      - Glossary: reference/glossary.md
      - Entity reference: reference/entities.md
      - Relationship reference: reference/relationships.md
      - Example payloads: reference/examples.md
```

# Proposed repository structure

```text
docs/
├── index.md
│
├── getting-started/
│   ├── overview.md
│   ├── architecture.md
│   ├── responsibilities.md
│   └── implementation-roadmap.md
│
├── data-model/
│   ├── index.md
│   ├── entity-relationships.md
│   ├── master-data.md
│   ├── production.md
│   ├── plan-and-actual.md
│   ├── measurements.md
│   └── traceability.md
│
├── integration/
│   ├── preparation.md
│   ├── master-data.md
│   ├── production.md
│   ├── sensor-data.md
│   ├── traceability.md
│   └── synchronization.md
│
├── scenarios/
│   ├── typical-production-day.md
│   ├── work-order-lifecycle.md
│   ├── machine-downtime.md
│   ├── sensor-capture.md
│   └── waste-and-quality.md
│
├── mapping/
│   ├── index.md
│   ├── erp.md
│   ├── mes.md
│   ├── scada-iot.md
│   └── template.md
│
├── api/
│   ├── index.md
│   ├── authentication.md
│   ├── conventions.md
│   ├── errors.md
│   └── reference.md
│
├── implementation/
│   ├── data-quality.md
│   ├── testing.md
│   ├── troubleshooting.md
│   └── go-live-checklist.md
│
├── reference/
│   ├── glossary.md
│   ├── entities.md
│   ├── relationships.md
│   └── examples.md
│
└── assets/
    ├── diagrams/
    ├── images/
    └── examples/
```

# Minimum viable documentation

The first public version should not attempt to cover every planned section.

A useful initial version could contain:

1. Integration overview
2. Integration architecture
3. PULSE data-model overview
4. Entity relationships
5. A typical production day
6. Mapping customer data to PULSE
7. Authentication
8. Initial API examples
9. Data-quality checklist
10. Glossary

This would provide a complete basic path:

```text
 Understand PULSE
 → Understand the data model
 → Understand the production scenario
 → Map the customer system
 → Send data through the API
 → Validate the integration
```
