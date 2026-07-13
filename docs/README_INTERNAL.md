# Pulse Integration Documentation – Internal Information

## Purpose of this repository

This repository contains the public technical documentation for integrating external customer systems with **Pulse**.

Pulse receives operational data from customer systems and uses it to generate analyses, signals, and recommendations.

The documentation must therefore do more than describe API endpoints. It should explain how customer records map to the Pulse data model, how related records are connected, and how integrators can send valid data with as little friction as possible.

> [!NOTE]
> For the current navigation and repository layout, see [Project Structure](ProjectStructure.md).

The documentation should help integrators understand:

* what data Pulse needs,
* where that data usually originates,
* how customer records map to Pulse entities,
* how related records are connected,
* how to authenticate and send data,
* how to validate the result,
* how updates and corrections are handled,
* and how data quality affects Pulse results.

The intended reader journey is:

```text
Understand the integration
→ Meet the requirements
→ Authenticate
→ Send the first data
→ Validate the result
→ Build the complete integration
```

## Target audience

The documentation is intended primarily for:

* customer integration teams,
* ERP integrators,
* MES integrators,
* SCADA and IoT integrators,
* solution architects,
* software developers,
* implementation consultants,
* internal Pulse developers,
* and internal domain experts.

The first public version should be task-oriented and concise. It should help a developer complete one small successful integration before introducing advanced concepts or the full data model.

## Documentation layers

The documentation should be divided into three practical layers.

### Getting started

Helps the developer complete the first successful API interaction.

Examples:

* integration overview,
* requirements,
* authentication,
* first request,
* result validation.

### Integration guidance

Explains how to build the real integration.

Examples:

* mapping customer data,
* synchronizing master data,
* sending operational activity,
* sending measurements,
* handling updates and errors.

### API reference

Explains the exact technical contract.

Examples:

* endpoints,
* authentication requirements,
* request and response schemas,
* required fields,
* status codes,
* validation rules,
* example payloads.

Swagger documentation will primarily support the API-reference layer. The task-oriented documentation must explain when and why to use the API operations.

# Minimum viable documentation

The first public version should contain only the pages needed to guide a developer through a working integration.

A useful initial version should include:

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

This provides the following basic path:

```text
Understand Pulse
→ Authenticate
→ Send one request
→ Confirm that it worked
→ Map the customer system
→ Build the complete integration
```

Detailed entity pages, advanced synchronization patterns, extensive testing theory, and full field references should be added only when they provide clear implementation value.

# Content-development approach

## Phase 1: Confirm the core model

* review the URS,
* identify the entities required for a minimal integration,
* confirm the meaning of Batch and Stage,
* document the relationships required for synchronization,
* identify unresolved terminology.

## Phase 2: Define the first working scenario

* select one small, safe API example,
* define the required source data,
* map the example to Pulse entities,
* document the expected request and response,
* define how the result is validated.

## Phase 3: Connect the workflow to the API

* review Swagger documentation,
* identify the relevant endpoints,
* create example requests and responses,
* document authentication,
* document required synchronization order,
* document update and correction behaviour.

## Phase 4: Add integration guidance

* customer-data mapping,
* identifiers,
* timestamps and time zones,
* units,
* master-data synchronization,
* operational activity,
* measurements,
* updates,
* retries,
* validation,
* troubleshooting.

## Phase 5: Publish and expand

* maintain the MkDocs site,
* publish the minimum viable documentation,
* add more examples only when confirmed,
* expand the API and data-model sections as implementation details become available.

# First-stage open questions

The following questions should be answered first because they directly affect the initial documentation and API examples:

1. **What does a Pulse batch represent?**  
   We know it is not limited to production, but we need a clear definition and valid examples.

2. **What does a Pulse stage represent?**  
   Is it always a step within a batch, and can it be used outside production?

3. **Which entities are required for the smallest valid integration?**  
   This determines what the first request should contain.

4. **How are customer records identified in Pulse?**  
   Does the customer send its own identifier, does Pulse generate one, or are both stored?

5. **In which order must data be sent?**  
   For example, must master data exist before batches, stages, measurements, or usage records?

6. **How are records created and later corrected?**  
   We need to know whether the API uses create, update, patch, or upsert behaviour.

7. **Which timestamp and time-zone rules apply?**  
   This is essential for activity, measurements, downtime, and usage periods.

8. **How are measurements linked to business activity?**  
   Are they linked directly to equipment, a batch, a stage, or inferred from timestamps?

9. **How should good output, rejected output, scrap, rework, and waste be represented?**  
   These concepts must not be mixed.

10. **Are Pulse recommendations only presented to users, or can Pulse also trigger actions in connected customer systems?**  
    This clarifies the output side of the integration and the value that can be demonstrated.

Additional questions should be added only when they become necessary during Swagger review or content development.

# Internal documentation principles

The public documentation should:

* be direct and task-oriented,
* help the reader complete a successful request early,
* explain only the concepts required for the current task,
* keep terminology consistent with the Pulse data model,
* avoid treating Batch as a production-only concept,
* use production as an example without implying that Pulse is limited to production,
* clearly separate master data and operational data,
* clearly separate planned and actual data where relevant,
* use source-to-Pulse mapping tables,
* include one complete end-to-end example,
* link API operations to business activities,
* distinguish confirmed behaviour from assumptions,
* avoid exposing internal-only implementation details,
* and remain understandable to integrators who are not familiar with Pulse.

The internal README and [Project Structure](ProjectStructure.md) should be updated as the structure evolves and open questions are resolved.