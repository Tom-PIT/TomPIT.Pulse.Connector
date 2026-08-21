# Reason

Represents a cause used to classify stoppages, deviations, maintenance, complaints, and other operational events in Food & Beverage operations.

Reasons can be organised hierarchically so detailed causes can roll up into broader cause groups or families.

## The Reason object

```json
{
  "code": "CMP-FOREIGN-BODY",
  "name": "Foreign body",
  "parent": "CMP-SAFETY",
  "types": {
    "reasonType": "COMPLAINT"
  },
  "attributes": {
    "sourceCode": "QMS-17"
  }
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the reason in source systems and integrations. | `"CMP-FOREIGN-BODY"` |
| `name` | string | Human-readable name of the reason. | `"Foreign body"` |
| `parent` | string or null | Optional code of the parent reason used to build a hierarchical reason tree. | `"CMP-SAFETY"` |
| `types` | object or null | Optional classifications used to group and analyse the reason. | `{ "reasonType": "COMPLAINT" }` |
| `attributes` | object or null | Optional additional source-system metadata associated with the reason. These values are stored but are not used for analysis. | `{ "sourceCode": "QMS-17" }` |

</div>

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## Reason hierarchy

Reasons can be arranged into parent-child relationships.

For example:

```text
CMP-SAFETY
├── CMP-FOREIGN-BODY
├── CMP-MICROBIOLOGICAL
├── CMP-ALLERGEN
└── CMP-CHEMICAL
```

A child reason references its parent using the `parent` field.

Using one shared reason hierarchy allows Pulse to compare causes across different operational records instead of maintaining separate cause vocabularies for each area.

## API resource

| Resource | Base path |
| --- | --- |
| Reason | `/services/pulse/food-beverage/reasons` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## Depends on

May reference another Reason as its parent.

When `parent` is provided, the referenced parent reason must be available before submitting the child reason.
