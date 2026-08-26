# Cleaning rule

Defines the cleaning regime required when production changes from one product to another.

A Cleaning rule also defines the expected duration of that changeover clean.

## The Cleaning rule object

```json
{
  "after": "PRD001",
  "before": "PRD044",
  "regime": "allergen-cip",
  "expectedMinutes": 95
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| [`after`](../master-data/product.md) | string | Business code of the product that finishes before cleaning. | `"PRD001"` |
| [`before`](../master-data/product.md) | string | Business code of the product that runs after cleaning. | `"PRD044"` |
| [`regime`](clean-regime.md) | string | Business code of the cleaning regime required for the changeover. | `"allergen-cip"` |
| `expectedMinutes` | integer | Expected duration of the cleaning operation, in minutes. | `95` |

</div>

> [!IMPORTANT]
> `after` and `before` must reference existing products, and `regime` must reference an existing cleaning regime.
>
> A Cleaning rule is directional. A rule from product A to product B does not automatically define the rule from product B to product A.

## Directional rules

Cleaning requirements can depend on the order in which products are produced.

For example, changing from an allergen-containing product to an allergen-free product may require a validated allergen clean, while the reverse changeover may require a different regime or duration.

Therefore:

```text
PRD001 → PRD044
```

and:

```text
PRD044 → PRD001
```

are two separate Cleaning rules.

The combination of `after` and `before` identifies the rule.

## Expected duration

`expectedMinutes` defines how long the cleaning operation is expected to take for the specified product transition.

For example:

```json
{
  "after": "PRD001",
  "before": "PRD044",
  "regime": "allergen-cip",
  "expectedMinutes": 95
}
```

Actual cleaning duration can later be compared with this expected value.

## API resource

| Resource | Base path |
| --- | --- |
| `Cleaning rule` | `/services/pulse/food-beverage/cleaning-rules` |

## API methods

### Create a cleaning rule

`POST /services/pulse/food-beverage/cleaning-rules/insert`

Creates a Cleaning rule.

If a rule with the same `after` and `before` values already exists, the existing rule is updated with the submitted `regime` and `expectedMinutes`.

#### Request

```http
POST /services/pulse/food-beverage/cleaning-rules/insert
Content-Type: application/json
```

```json
{
  "after": "PRD001",
  "before": "PRD044",
  "regime": "allergen-cip",
  "expectedMinutes": 95
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `after` | string | yes | Business code of the product that finishes before cleaning. |
| `before` | string | yes | Business code of the product that runs after cleaning. |
| `regime` | string | yes | Business code of the required cleaning regime. |
| `expectedMinutes` | integer | yes | Expected cleaning duration in minutes. |

### Update a cleaning rule

`PUT /services/pulse/food-beverage/cleaning-rules/update`

Updates an existing Cleaning rule identified by its `after` and `before` values.

#### Request

```http
PUT /services/pulse/food-beverage/cleaning-rules/update
Content-Type: application/json
```

```json
{
  "after": "PRD001",
  "before": "PRD044",
  "regime": "allergen-cip",
  "expectedMinutes": 100
}
```

### Patch a cleaning rule

`PATCH /services/pulse/food-beverage/cleaning-rules/patch`

Partially updates an existing Cleaning rule.

The fields to update are supplied in the `properties` object. Both `after` and `before` are required to identify the rule.

#### Request

```http
PATCH /services/pulse/food-beverage/cleaning-rules/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "after": "PRD001",
    "before": "PRD044",
    "expectedMinutes": 100
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.after` | string | yes | Business code of the product that finishes before cleaning. |
| `properties.before` | string | yes | Business code of the product that runs after cleaning. |
| `properties.regime` | string | no | New cleaning-regime business code. |
| `properties.expectedMinutes` | integer | no | New expected cleaning duration in minutes. |

### Retrieve a cleaning rule

`GET /services/pulse/food-beverage/cleaning-rules/select`

Returns the Cleaning rule identified by its `after` and `before` values.

#### Request

```http
GET /services/pulse/food-beverage/cleaning-rules/select?after=PRD001&before=PRD044
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `after` | string | yes | Business code of the product that finishes before cleaning. |
| `before` | string | yes | Business code of the product that runs after cleaning. |

#### Example response

```json
{
  "after": "PRD001",
  "before": "PRD044",
  "regime": "allergen-cip",
  "expectedMinutes": 95
}
```

### List cleaning rules

`GET /services/pulse/food-beverage/cleaning-rules/query`

Returns Cleaning rules matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/cleaning-rules/query?afters=PRD001&regimes=allergen-cip
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `afters` | string or array of strings | no | Limits results to rules with the specified `after` product codes. |
| `befores` | string or array of strings | no | Limits results to rules with the specified `before` product codes. |
| `regimes` | string or array of strings | no | Limits results to rules using the specified cleaning-regime codes. |

#### Example response

```json
[
  {
    "after": "PRD001",
    "before": "PRD044",
    "regime": "allergen-cip",
    "expectedMinutes": 95
  }
]
```

### Delete a cleaning rule

`DELETE /services/pulse/food-beverage/cleaning-rules/delete`

Deletes the Cleaning rule identified by its `after` and `before` values.

#### Request

```http
DELETE /services/pulse/food-beverage/cleaning-rules/delete?after=PRD001&before=PRD044
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `after` | string | yes | Business code of the product that finishes before cleaning. |
| `before` | string | yes | Business code of the product that runs after cleaning. |