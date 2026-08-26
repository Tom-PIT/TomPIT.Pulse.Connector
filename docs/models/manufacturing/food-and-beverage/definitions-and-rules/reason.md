# Reason

Represents a hierarchical reason used to explain operational stops, deviations, complaints, and other operational events.

Reasons can be organised hierarchically with no fixed depth.

## The Reason object

```json
{
  "code": "CMP-FOREIGN-BODY",
  "name": "Foreign body",
  "parent": "CMP-SAFETY"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Unique business code used to identify the reason. | `"CMP-FOREIGN-BODY"` |
| `name` | string | Human-readable name of the reason. | `"Foreign body"` |
| `parent` | string or null | Business code of the parent reason. Omit for a top-level reason. | `"CMP-SAFETY"` |

</div>

> [!IMPORTANT]
> `code` must be unique. Two reasons cannot use the same code.
>
> When `parent` is provided, the referenced parent reason must already exist.
>
> A reason cannot be assigned a parent that would create a cycle in the hierarchy.

## Reason hierarchy

Reasons form a tree with no fixed hierarchy depth.

For example:

```text
Complaint
└── Safety
    └── Foreign body
```

A reason references the level directly above it through `parent`.

The same hierarchy can be used across stoppages, deviations, complaints, and other operational records.

A reason that has child reasons cannot be deleted until those child references are removed or reassigned.

## API resource

| Resource | Base path |
| --- | --- |
| `Reason` | `/services/pulse/food-beverage/reasons` |

## API methods

### Create a reason

`POST /services/pulse/food-beverage/reasons/insert`

Creates a new reason.

#### Request

```http
POST /services/pulse/food-beverage/reasons/insert
Content-Type: application/json
```

```json
{
  "code": "CMP-FOREIGN-BODY",
  "name": "Foreign body",
  "parent": "CMP-SAFETY"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the reason. |
| `name` | string | yes | Human-readable name of the reason. |
| `parent` | string or null | no | Business code of the parent reason. |

### Update a reason

`PUT /services/pulse/food-beverage/reasons/update`

Updates an existing reason identified by its business `code`.

#### Request

```http
PUT /services/pulse/food-beverage/reasons/update
Content-Type: application/json
```

```json
{
  "code": "CMP-FOREIGN-BODY",
  "name": "Foreign body contamination",
  "parent": "CMP-SAFETY"
}
```

### Patch a reason

`PATCH /services/pulse/food-beverage/reasons/patch`

Partially updates an existing reason.

The reason is identified by `properties.code`. The current implementation also requires `properties.name`. The `parent` field can be supplied when the hierarchy should change.

#### Request

```http
PATCH /services/pulse/food-beverage/reasons/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "CMP-FOREIGN-BODY",
    "name": "Foreign body contamination"
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Business code of the reason to update. |
| `properties.name` | string | yes | Human-readable name of the reason. |
| `properties.parent` | string or null | no | New parent reason. Use `null` to make the reason top-level. |

### Retrieve a reason

`GET /services/pulse/food-beverage/reasons/select`

Returns the reason identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/reasons/select?id=CMP-FOREIGN-BODY
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the reason to retrieve. |

#### Example response

```json
{
  "code": "CMP-FOREIGN-BODY",
  "name": "Foreign body",
  "parent": "CMP-SAFETY"
}
```

### List reasons

`GET /services/pulse/food-beverage/reasons/query`

Returns reasons matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/reasons/query?parents=CMP-SAFETY
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `codes` | string or array of strings | no | Limits results to reasons with the specified business codes. |
| `names` | string or array of strings | no | Limits results to reasons with the specified names. |
| `parents` | string or array of strings | no | Limits results to reasons with the specified parent reason codes. An empty value can be used to include top-level reasons. |

#### Example response

```json
[
  {
    "code": "CMP-FOREIGN-BODY",
    "name": "Foreign body",
    "parent": "CMP-SAFETY"
  }
]
```

### Delete a reason

`DELETE /services/pulse/food-beverage/reasons/delete`

Deletes the reason identified by its business code.

A reason cannot be deleted while child reasons still reference it.

#### Request

```http
DELETE /services/pulse/food-beverage/reasons/delete?id=CMP-FOREIGN-BODY
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the reason to delete. |