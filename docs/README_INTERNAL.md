# PULSE Integration Documentation – Internal Information

## Purpose of this repository

This repository will contain the public technical documentation for integrating external customer systems with **PULSE**.

PULSE receives operational and production data from customer systems and uses that data to generate analyses, signals, and recommendations.

The purpose of the documentation is therefore not only to describe API endpoints. It must explain how real production processes and source-system records are translated into the PULSE data model.


> [!NOTE]
> For more information on project structure, see [Project Structure](ProjectStructure.md).

The documentation should help integrators understand:

* which data PULSE needs,
* where that data typically originates,
* how source-system records map to PULSE entities,
* how different records are related,
* in which order data should be synchronized,
* how data should be sent through the API,
* how updates and corrections should be handled,
* and how data quality affects the value produced by PULSE.

The main documentation flow should be:

```text
Customer production process
→ Source systems
→ Data mapping
→ PULSE API
→ PULSE data model
→ Analysis and recommendations
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
* internal PULSE developers,
* and internal domain experts.

Different readers will need different levels of detail.

An integration architect may first need to understand the overall production and data model, while a developer will eventually need exact API endpoints, payloads, validation rules, and error-handling instructions.

For this reason, the documentation should be divided into three main layers:

### Conceptual documentation

Explains what the data represents and how the entities are related.

Examples:

* what a batch represents,
* what a production stage represents,
* what the difference is between planned and actual data,
* how machines, labour, materials, output, waste, and downtime are connected.

### Integration guidance

Explains what the customer or integrator needs to implement.

Examples:

* how to identify source systems,
* how to map identifiers,
* how to synchronize master data,
* how to send production transactions,
* how to handle delayed or corrected records.

### API reference

Explains the exact technical implementation.

Examples:

* endpoints,
* authentication,
* request payloads,
* response payloads,
* required fields,
* validation rules,
* error responses.

Swagger documentation will primarily support the API-reference layer. The remaining documentation must explain the business context and integration process around the API.

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

# Content-development approach

The documentation should be developed in the following order.

## Phase 1: Understand the model

* review the URS,
* identify all relevant PULSE entities,
* document their relationships,
* identify unresolved terminology,
* confirm unclear relationships with domain experts.

## Phase 2: Define the production story

* describe a realistic production day,
* identify the systems that create each record,
* map each event to a PULSE entity,
* identify missing information,
* confirm the intended PULSE interpretation.

## Phase 3: Connect the story to the API

* review Swagger documentation,
* identify relevant endpoints,
* connect each production event to an API operation,
* create example requests and responses,
* document required synchronization order.

## Phase 4: Add implementation guidance

* authentication,
* synchronization,
* identifiers,
* timestamps,
* units,
* corrections,
* retries,
* validation,
* data quality,
* testing.

## Phase 5: Publish and expand

* create the public GitHub repository,
* configure MkDocs Material,
* configure GitHub Pages,
* publish the minimum viable documentation,
* expand individual entity and API pages over time.

# Open questions

The following points require confirmation before the related documentation can be finalized:

* Does a customer work order always map to a PULSE batch?
* Does a work-order operation always map to a PULSE stage?
* Who generates PULSE entity identifiers?
* Are customer identifiers preserved directly?
* How are records updated or corrected?
* Is the API based on create, update, or upsert operations?
* How are duplicate requests detected?
* Is record deletion supported?
* How are inactive master-data records handled?
* In which order must entities be synchronized?
* Which timestamp format and time-zone rules apply?
* Which units and currencies are supported?
* Which values are allowed for measurement dimensions?
* How are measurements linked to current production?
* Does labour represent individual employees, roles, teams, or all three?
* How should rejected production, scrap, rework, and waste be distinguished?
* How should downtime, delays, and maintenance events be distinguished?
* Which analyses and recommendations can customers retrieve through the API?
* Which parts of the Swagger reference will be embedded, linked, or documented manually?

 These questions should be tracked and answered through API review and discussions with the relevant domain experts.

# Internal documentation principles

The public documentation should:

* explain concepts before presenting endpoints,
* use realistic production examples,
* keep terminology consistent with the PULSE data model,
* identify common customer terminology where it differs,
* clearly separate planned and actual data,
* clearly separate master data and transactional data,
* show relationships visually whenever useful,
* include source-to-PULSE mapping tables,
* include complete end-to-end examples,
* link API operations to production events,
* distinguish confirmed behaviour from assumptions,
* avoid exposing internal-only implementation details,
* and remain understandable to integrators who are not familiar with PULSE.

The internal README and [Project Structure](ProjectStructure.md) should be updated as the structure evolves and as open questions are resolved.
