# Pulse integration documentation structure

This document defines the initial public structure for the Pulse integration documentation.

The first version should be concise and task-oriented. It should help an integrator complete a small successful integration before introducing the full Pulse data model or advanced implementation details.

For information about the project’s purpose and target audience, see [README_INTERNAL.md](README_INTERNAL.md).

## Documentation approach

The initial documentation should guide the reader through this path:

```text
Understand the integration
→ Prepare access
→ Authenticate
→ Send the first data
→ Validate the result
→ Build the complete integration
```

The documentation should:

- explain only the concepts required for the current task,
- place practical instructions before detailed theory,
- use one complete operational example,
- link business activities to Pulse entities and API operations,
- keep detailed field and endpoint information in the API reference,
- and move advanced topics into dedicated pages only when enough confirmed information is available.

## Initial public navigation

```yaml
nav:
  - Home: index.md

  - Get started:
      - Integration overview: getting-started/index.md
      - Authenticate: getting-started/authentication.md
      - Send your first data: getting-started/first-request.md
      - Validate the result: getting-started/validation.md

  - Integration workflow:
      - Map your data: integration/index.md
      - Synchronize master data: integration/master-data.md
      - Send operational activity: integration/operational-data.md
      - Send measurements: integration/measurements.md
      - Handle updates and errors: integration/updates-and-errors.md

  - Example:
      - Typical operational day: scenarios/typical-operational-day.md

  - Data model:
      - Core concepts: data-model/index.md
      - Entity relationships: data-model/relationships.md

  - API reference: api/index.md

  - Troubleshooting: troubleshooting.md
```

## Initial repository structure

```text
docs/
├── index.md
│
├── getting-started/
│   ├── index.md
│   ├── authentication.md
│   ├── first-request.md
│   └── validation.md
│
├── integration/
│   ├── index.md
│   ├── master-data.md
│   ├── operational-data.md
│   ├── measurements.md
│   └── updates-and-errors.md
│
├── scenarios/
│   └── typical-operational-day.md
│
├── data-model/
│   ├── index.md
│   └── relationships.md
│
├── api/
│   └── index.md
│
├── troubleshooting.md
│
└── assets/
    ├── diagrams/
    ├── images/
    └── examples/
```

## Page purpose

### Home

The home page should briefly explain:

- what Pulse is,
- what the integration does,
- who the documentation is for,
- and where a new integrator should begin.

It should not contain detailed data-model or API information.

### Get started

This section should help a developer make the first successful API request.

#### Integration overview

Explains:

- how customer data reaches Pulse,
- typical source systems,
- the minimum integration requirements,
- the responsibilities of the customer, integrator, and Pulse,
- and the high-level implementation sequence.

The requirements remain inside this page unless they later become detailed enough to require a separate article.

#### Authenticate

Explains:

- how credentials are obtained,
- how authentication is added to a request,
- which environments are available,
- and how to verify that authentication works.

#### Send your first data

Provides the smallest complete request that can be sent safely.

The page should include:

- the purpose of the request,
- the endpoint,
- required headers,
- a minimal payload,
- an example response,
- and common mistakes.

The exact example will be selected after the Pulse Swagger definition is reviewed.

#### Validate the result

Explains how to confirm that:

- the request was accepted,
- the record was stored or processed,
- referenced entities were resolved,
- and the submitted data is usable by Pulse.

### Integration workflow

This section explains how to move from the first successful request to a complete integration.

#### Map your data

Explains how to:

- identify source systems,
- identify authoritative records,
- map customer concepts to Pulse entities,
- define identifiers,
- map units, timestamps, statuses, and enumerations,
- and document unresolved mappings.

A Pulse batch must not be described only as a production order. It is a broader unit of business activity and may be used in production, supply, logistics, or another operational context.

#### Synchronize master data

Explains which reference records generally need to exist before operational records are sent.

Examples may include:

- organizational structures,
- plants and locations,
- equipment,
- products and materials,
- customers and suppliers,
- shifts,
- measurement types,
- downtime classifications,
- and waste classifications.

The final list and synchronization order must be confirmed against the API.

#### Send operational activity

Explains how to send business activity and execution records.

Depending on the supported Pulse model, this may include:

- batches,
- stages,
- plans,
- actual activity,
- labour and equipment usage,
- material and energy usage,
- quantities and results,
- downtime,
- delays,
- waste,
- and expenses.

Production should be used as the primary example, but the documentation should not imply that Pulse is limited to production.

#### Send measurements

Explains how to send sensor, machine, process, and environmental measurements.

The page should cover:

- measurement type,
- value and unit,
- timestamp,
- related Pulse entity,
- sampling or aggregation rules,
- and how measurements receive operational context.

#### Handle updates and errors

Explains:

- updates and corrections,
- repeated requests,
- duplicate prevention,
- retries,
- validation failures,
- missing references,
- inactive or deleted records,
- and recovery after interrupted synchronization.

### Example

#### Typical operational day

This page should provide one complete end-to-end scenario.

Production may be used as the first example because it is well represented in the current URS, but the scenario should explain that the same core concepts may apply to other operational domains.

Each step should show:

| Perspective | Description |
| --- | --- |
| Business activity | What happened in the company |
| Source system | Which system recorded it |
| Pulse model | Which Pulse entity represents it |
| API | Which operation sends it |

A possible production-oriented sequence is:

```text
1. A business or production activity is created.
2. The activity is planned.
3. Its stages or steps are defined.
4. Execution begins.
5. Labour, equipment, materials, and measurements are recorded.
6. A delay or downtime event occurs.
7. Execution resumes.
8. Results, quantities, waste, or costs are recorded.
9. The activity is completed.
10. Pulse evaluates the complete context.
```

### Data model

#### Core concepts

Provides only the concepts required to understand the integration workflow.

Initial concepts may include:

- batch,
- stage,
- plan and actual activity,
- master data,
- usage records,
- measurements,
- downtime and delays,
- waste,
- traceability,
- and entity identifiers.

Detailed entity-by-entity descriptions should not be added unless they are needed outside the API reference.

#### Entity relationships

Shows how the main Pulse entities are connected.

The page should focus on relationships that affect:

- synchronization order,
- required references,
- data mapping,
- and validation.

### API reference

The API reference should contain or link to:

- Swagger or OpenAPI documentation,
- endpoints,
- request parameters,
- schemas,
- required fields,
- example requests and responses,
- authentication requirements,
- status codes,
- and validation errors.

Conceptual guidance should remain in the task-oriented pages rather than being duplicated in the API reference.

### Troubleshooting

This page should provide short, practical solutions for common integration problems, including:

- authentication failures,
- invalid requests,
- missing references,
- duplicate records,
- incorrect identifiers,
- incorrect timestamps or units,
- incomplete operational context,
- measurement problems,
- and retry failures.

## Minimum viable documentation

The first useful public version should contain:

1. Integration overview
2. Authentication
3. First API request
4. Result validation
5. Data mapping
6. Master-data synchronization
7. Operational activity
8. Measurement submission
9. One complete operational example
10. Core data-model concepts
11. Entity relationships
12. API reference
13. Troubleshooting

This provides the following reader journey:

```text
Understand Pulse
→ Meet the requirements
→ Authenticate
→ Send one request
→ Confirm that it worked
→ Map the customer system
→ Build the complete integration
```

## Future expansion

The documentation may later add dedicated pages for:

- additional operational scenarios,
- advanced synchronization patterns,
- detailed traceability,
- high-frequency measurements,
- testing and go-live checklists,
- mapping templates,
- complete entity reference,
- field reference,
- enumerations,
- SDK examples,
- webhooks or event notifications,
- and changelogs.

These pages should be added only when they provide clear implementation value and enough confirmed information is available.