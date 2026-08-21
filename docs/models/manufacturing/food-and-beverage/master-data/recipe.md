# Recipe

Represents a formulation version used in Food & Beverage production.

A recipe is separate from a product because a change in formulation represents a new production regime. Register each formulation version with its own recipe code.

## The Recipe object

```json
{
  "code": "REC-YOG-STRAWBERRY-V3",
  "name": "Strawberry Yogurt Recipe v3",
  "types": {
    "recipeFamily": "STRAWBERRY-YOGURT"
  },
  "attributes": {
    "revision": "3"
  }
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the recipe or formulation version in source systems and integrations. | `"REC-YOG-STRAWBERRY-V3"` |
| `name` | string | Human-readable name of the recipe or formulation version. | `"Strawberry Yogurt Recipe v3"` |
| `types` | object or null | Optional classifications used to group and analyse the recipe. | `{ "recipeFamily": "STRAWBERRY-YOGURT" }` |
| `attributes` | object or null | Optional additional source-system metadata associated with the recipe. These values are stored but are not used for analysis. | `{ "revision": "3" }` |

</div>

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## Recipe versions

Register a new recipe when the formulation changes in a way that should distinguish production before and after the change.

For example:

```text
REC-YOG-STRAWBERRY-V2
REC-YOG-STRAWBERRY-V3
```

Keeping formulation versions separate allows Pulse to compare production performed under different recipes.

## API resource

| Resource | Base path |
| --- | --- |
| Recipe | `/services/pulse/food-beverage/recipes` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.
