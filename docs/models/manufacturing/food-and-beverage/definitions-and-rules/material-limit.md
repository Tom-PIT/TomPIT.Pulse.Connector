<!-- Draft: not currently supported -->
<!-- TODO: This page is currently based on ApiSurfaceRevised. Revisit it once the implementation is available and verify fields, routes, query parameters, PATCH behavior, and examples against the current code. -->

# Material limits

Defines incoming material specifications that would apply to a material from a specific point in time.

Material limits use the same general shape as [Product limits](product-limits.md), but reference a material instead of a product and do not support `stopsRelease`.

> [!WARNING]
> Material limits are not currently supported.
>
> Requests to this resource are rejected with `no-active-profile`. The resource is documented so integrations can prepare for future support without silently accepting limits that Pulse cannot yet evaluate.

## The Material limit object

```json
{
  "material": "MAT0001",
  "measure": "protein",
  "min": 3.1,
  "unit": "%",
  "from": "2026-01-01T00:00:00+01:00"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| [`material`](../master-data/material.md) | string | Business code of the material to which the limit applies. | `"MAT0001"` |
| [`measure`](measurement.md) | string | Measurement code that the limit applies to. | `"protein"` |
| `min` | number or null | Optional minimum permitted or expected value. | `3.1` |
| `target` | number or null | Optional target value. | `3.3` |
| `max` | number or null | Optional maximum permitted or expected value. | `3.5` |
| `unit` | string | Unit of the submitted values. Used to validate the values against the measurement definition. | `"%"` |
| `from` | string | ISO 8601 timestamp from which the limit would be in force. | `"2026-01-01T00:00:00+01:00"` |
| `setBy` | string or null | Optional source or authority that established the limit. | `"Supplier specification rev 2"` |

</div>

> [!IMPORTANT]
> At least one of `min`, `target`, or `max` must be provided.
>
> `material` must reference an existing material and `measure` must reference an existing measurement.
>
> `unit` must be compatible with the unit declared for the measurement.

See [Types and attributes](../master-data/types-and-attributes.md) for guidance on extensible master-data properties.

## Current limitation

Product specifications can currently use a product as their subject, but material specifications are not yet active.

Submitting a material limit is therefore rejected rather than accepted and ignored.

Example error:

```json
{
  "error": "no-active-profile",
  "detail": "Material specifications are not currently supported."
}
```

## API resource

| Resource | Base path |
| --- | --- |
| `Material limit` | `/services/pulse/food-beverage/material-limits` |

## API methods

> [!NOTE]
> The API method shape below is provisional. The resource is currently unsupported and requests are expected to be rejected.

### Create a material limit

`POST /services/pulse/food-beverage/material-limits/insert`

Attempts to create a material limit.

#### Request

```http
POST /services/pulse/food-beverage/material-limits/insert
Content-Type: application/json
```

```json
{
  "material": "MAT0001",
  "measure": "protein",
  "min": 3.1,
  "unit": "%",
  "from": "2026-01-01T00:00:00+01:00"
}
```

#### Current behaviour

The request is rejected with `no-active-profile`.