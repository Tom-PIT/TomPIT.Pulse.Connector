# API reference

The **Pulse API** is a REST API for submitting Food & Beverage integration data and retrieving Pulse results.

This page provides an index of the public API resource groups. Use Scalar for the complete operation-level reference, including request fields, parameters, schemas, responses, and the exact operations supported by each resource.

<div class="grid cards" markdown>

- [**Open the complete API reference in Scalar**](#)

</div>

## API groups

<div class="grid cards" markdown>

- [**Master data**](#master-data)
- [**Definitions and rules**](#definitions-and-rules)
- [**Production activities**](#production-activities)
- [**Operational data**](#operational-data)
- [**Maintenance and quality**](#maintenance-and-quality)

</div>

## Using this reference

Use this page to identify the relevant API resource and its base path.

Open the resource documentation for integration guidance and examples. Use Scalar to inspect the complete generated API contract.

## API conventions

### Base route

Food & Beverage resources use the following base route:

```text
{host}/services/pulse/food-beverage/{resource}
```

For example:

```text
/services/pulse/food-beverage/products
/services/pulse/food-beverage/runs
/services/pulse/food-beverage/readings
```

### Business codes

Food & Beverage integrations use business codes rather than Pulse internal numeric identifiers.

For example:

```json
{
  "code": "YOG-RUN-001",
  "line": "YOGURT-LINE-01",
  "product": "YOG-STRAWBERRY-150G"
}
```

References to related records also use their business codes.

### Operations

Resources implemented through the current Food & Beverage service pattern use operation-specific paths such as:

```text
POST   /{resource}/insert
PUT    /{resource}/update
PATCH  /{resource}/patch
GET    /{resource}/select
GET    /{resource}/query
DELETE /{resource}/delete
```

The operations available for individual resources may differ. See the resource documentation and Scalar for the exact contract.

### Timestamps

Use ISO 8601 timestamps with an explicit UTC offset where a date and time is required.

For example:

```text
2026-08-10T22:00:00+02:00
```

Some fields may also accept a date where the resource explicitly defines one.

---

## Master data

Master data contains the relatively stable business records referenced by Food & Beverage integrations.

See [Master data](../master-data/index.md).

| Resource | Description | Base path |
| --- | --- | --- |
| [**Site**](../master-data/site.md) | Physical operating location. | `/services/pulse/food-beverage/plants` |
| [**Production line**](../master-data/production-line.md) | Production line within a site. | `/services/pulse/food-beverage/lines` |
| [**Machine**](../master-data/machine.md) | Machine, component, sensor, or wear part. | `/services/pulse/food-beverage/machines` |
| [**Vessel**](../master-data/vessel.md) | Tank, silo, or other process vessel. | `/services/pulse/food-beverage/vessels` |
| [**Product**](../master-data/product.md) | Finished product or other tracked production output. | `/services/pulse/food-beverage/products` |
| [**Recipe**](../master-data/recipe.md) | Recipe or formulation version used during production. | `/services/pulse/food-beverage/recipes` |
| [**Material**](../master-data/material.md) | Ingredient, packaging material, chemical, or other production material. | `/services/pulse/food-beverage/materials` |
| [**Cost line**](../master-data/cost-line.md) | Non-material cost used in production or maintenance. | `/services/pulse/food-beverage/cost-lines` |
| [**Supplier**](../master-data/supplier.md) | Supplier associated with materials and incoming lots. | `/services/pulse/food-beverage/suppliers` |
| [**Customer**](../master-data/customer.md) | Customer associated with Food & Beverage operations. | `/services/pulse/food-beverage/customers` |
| [**Shift**](../master-data/shift.md) | Work period used as production context. | `/services/pulse/food-beverage/shifts` |
| [**Crew**](../master-data/crew.md) | Team used as Labor and production context. | `/services/pulse/food-beverage/crews` |

See [Types and attributes](../master-data/types-and-attributes.md) for guidance on controlled classifications and extensible master-data properties.

---

## Definitions and rules

Definitions and rules describe measurements, limits, classifications, and operational rules used by other Food & Beverage resources.

See [Definitions and rules](../definitions-and-rules/index.md).

| Resource | Description | Base path |
| --- | --- | --- |
| [**Measurements**](../definitions-and-rules/measurements.md) | Defines measurements and setpoints accepted by Pulse. | `/services/pulse/food-beverage/measurements` |
| [**Product limits**](../definitions-and-rules/product-limit.md) | Defines time-effective product specification limits. | `/services/pulse/food-beverage/product-limits` |
| [**Targets**](../definitions-and-rules/targets.md) | Defines expected operating ranges or target values. | `/services/pulse/food-beverage/targets` |
| [**Clean regimes**](../definitions-and-rules/clean-regime.md) | Defines cleaning regimes used by cleaning activities and rules. | `/services/pulse/food-beverage/clean-regimes` |
| [**Cleaning rules**](../definitions-and-rules/cleaning-rule.md) | Defines the cleaning required between two products. | `/services/pulse/food-beverage/cleaning-rules` |
| [**Reasons**](../definitions-and-rules/reason.md) | Defines hierarchical reason codes used across operational records. | `/services/pulse/food-beverage/reasons` |
| [**Types**](../definitions-and-rules/type.md) | Defines controlled classifications and their allowed values. | `/services/pulse/food-beverage/types` |

---

## Production activities

Production activities describe the work performed during production, from incoming material lots through bulk processing, filling, packing, cleaning, and quality holds.

See [Production activities](../production-activities/index.md).

| Resource | Description | Base path |
| --- | --- | --- |
| [**Lot**](../production-activities/lot.md) | Records a traceable quantity of material received from a supplier. | `/services/pulse/food-beverage/lots` |
| [**Batch**](../production-activities/batch.md) | Records bulk production such as cooking, fermentation, or blending. | `/services/pulse/food-beverage/batches` |
| [**Run**](../production-activities/run.md) | Records filling or packing production on a production line. | `/services/pulse/food-beverage/runs` |
| [**Planned use**](../production-activities/planned-use.md) | Defines resources a run, batch, or clean is expected to use. | `/services/pulse/food-beverage/planned-use` |
| [**Stage**](../production-activities/stage.md) | Records an execution step within a production run. | `/services/pulse/food-beverage/stages` |
| [**Clean**](../production-activities/clean.md) | Records cleaning work between two products. | `/services/pulse/food-beverage/cleans` |
| [**Hold**](../production-activities/hold.md) | Records a quality hold placed on finished stock. | `/services/pulse/food-beverage/holds` |

---

## Operational data

Operational data records what actually happened during production.

See [Operational data](../operational-data/index.md).

| Resource | Description | Base path |
| --- | --- | --- |
| [**Consumption**](../operational-data/consumption.md) | Records resources actually used by a run, batch, or clean. | `/services/pulse/food-beverage/consumption` |
| [**Output**](../operational-data/output.md) | Records good output, waste, downgrade, and reject quantities. | `/services/pulse/food-beverage/output` |
| [**Readings**](../operational-data/reading.md) | Records measured values captured during production. | `/services/pulse/food-beverage/readings` |
| [**Settings**](../operational-data/settings.md) | Records commanded or configured machine values. | `/services/pulse/food-beverage/settings` |
| [**Line time**](../operational-data/line-time.md) | Records non-running or constrained production-line intervals. | `/services/pulse/food-beverage/line-time` |
| [**Events**](../operational-data/event.md) | Records discrete operational occurrences. | `/services/pulse/food-beverage/events` |

---

## Maintenance and quality

Maintenance and quality resources cover maintenance work, resources used during maintenance, and customer complaints linked back to production.

See [Maintenance and quality](../maintenance-and-quality/index.md).

| Resource | Description | Base path |
| --- | --- | --- |
| [**Work orders**](../maintenance-and-quality/work-order.md) | Records preventive or corrective maintenance work. | `/services/pulse/food-beverage/work-orders` |
| [**Parts and Labor**](../maintenance-and-quality/parts-and-labour.md) | Records planned and actual maintenance resource use. | `/services/pulse/food-beverage/parts-and-labour` |
| [**Complaints**](../maintenance-and-quality/complaint.md) | Records customer complaints linked to products and traceable finished lots. | `/services/pulse/food-beverage/complaints` |