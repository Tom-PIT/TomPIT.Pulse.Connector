# Cleaning rules

<!-- TODO: This page is currently based on ApiSurfaceRevised. Revisit it once the implementation is available and verify fields, routes, query parameters, PATCH behavior, and examples against the current code. -->

Defines the cleaning regime required when production changes from one product to another.

A cleaning rule also defines the expected duration of that changeover clean.

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
| [`after`](../master-data/product.md) | string | Business code of the product that has just finished production. | `"PRD001"` |
| [`before`](../master-data/product.md) | string | Business code of the product that will run next. | `"PRD044"` |
| [`regime`](clean-regime.md) | string | Business code of the cleaning regime required for the changeover. | `"allergen-cip"` |
| `expectedMinutes` | number | Expected duration of the cleaning operation, in minutes. | `95` |

</div>

> [!IMPORTANT]
> `after`, `before`, and `regime` must reference existing records.
>
> A cleaning rule is directional. A rule from product A to product B does not automatically define the rule from product B to product A.

See [Types and attributes](../master-data/types-and-attributes.md) for guidance on extensible master-data properties.

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

are two separate cleaning rules.

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

> [!NOTE]
> The API methods below follow the current Food & Beverage service pattern and are provisional until the Cleaning rules implementation is available for verification.

### Create a cleaning rule

`POST /services/pulse/food-beverage/cleaning-rules/insert`

Creates a new cleaning rule.

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
| `after` | string | yes | Business code of the product that has just finished. |
| `before` | string | yes | Business code of the product that will run next. |
| `regime` | string | yes | Business code of the required cleaning regime. |
| `expectedMinutes` | number | yes | Expected cleaning duration in minutes. |


### Update a cleaning rule

`PUT /services/pulse/food-beverage/cleaning-rules/update`

Updates an existing cleaning rule.

The rule is identified by the combination of `after` and `before`.

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

Partially updates an existing cleaning rule.

The exact PATCH identification shape still needs to be verified against the implementation.

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


### Retrieve a cleaning rule

`GET /services/pulse/food-beverage/cleaning-rules/select`

Returns a cleaning rule.

The facade specification defines the rule key as the combination of `after` and `before`.

#### Request

```http
GET /services/pulse/food-beverage/cleaning-rules/select?after=PRD001&before=PRD044
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `after` | string | yes | Business code of the product that has just finished. |
| `before` | string | yes | Business code of the product that runs next. |

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

Returns cleaning rules matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/cleaning-rules/query?after=PRD001&regime=allergen-cip
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `after` | string | no | Limits results to rules starting from the specified product. |
| `before` | string | no | Limits results to rules ending with the specified product. |
| `regime` | string | no | Limits results to rules using the specified cleaning regime. |


### Delete a cleaning rule

`DELETE /services/pulse/food-beverage/cleaning-rules/delete`

Deletes a cleaning rule.

#### Request

```http
DELETE /services/pulse/food-beverage/cleaning-rules/delete?after=PRD001&before=PRD044
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `after` | string | yes | Business code of the product that has just finished. |
| `before` | string | yes | Business code of the product that runs next. |