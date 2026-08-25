# Stage

<!-- TODO: This page is currently based on ApiSurfaceRevised. Revisit it once the implementation is available and verify fields, routes, query parameters, PATCH behavior, and examples against the current code. -->

Represents a defined execution step within a production [Run](run.md).

A stage records when a specific part of the run was active and which machine performed that step.

Examples include filling, capping, labelling, or case packing.

## The Stage object

```json
{
  "run": "L01-260810-002",
  "stage": "fill",
  "machine": "EQ010",
  "start": "2026-08-10T22:10:00+02:00",
  "end": "2026-08-11T05:50:00+02:00"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| [`run`](run.md) | string | Business code of the production run to which the stage belongs. | `"L01-260810-002"` |
| `stage` | string | Stage code that identifies the production step within the run. | `"fill"` |
| [`machine`](../master-data/machine.md) | string | Business code of the machine performing the stage. | `"EQ010"` |
| `start` | string | Date and time when the stage started, in ISO 8601 format. | `"2026-08-10T22:10:00+02:00"` |
| `end` | string or null | Optional date and time when the stage ended. | `"2026-08-11T05:50:00+02:00"` |

</div>

> [!IMPORTANT]
> `run` and `stage` together identify the stage.
>
> `run` and `machine` must reference existing records.

## Stage identity

A stage is identified within its run by the combination of `run` and `stage`.

For example:

```text
L01-260810-002/fill
```

and:

```text
L01-260810-003/fill
```

represent different stage executions even though both use the `fill` stage code.

## Stages and machines

The machine is associated with the stage during which it operates rather than with the entire run.

For example:

```text
Run
├── fill   → EQ010
├── cap    → EQ020
├── label  → EQ030
└── case   → EQ040
```

This allows Pulse to preserve the actual operating period of each machine.

A production run may last eight hours while a particular machine operates for only part of that time. Recording the machine on the stage preserves that distinction.

## API resource

| Resource | Base path |
| --- | --- |
| `Stage` | `/services/pulse/food-beverage/stages` |

## API methods

> [!NOTE]
> The API methods below follow the current Food & Beverage service pattern and are provisional until the Stages implementation is available for verification.

### Create a stage

`POST /services/pulse/food-beverage/stages/insert`

Creates a stage within a production run.

#### Request

```http
POST /services/pulse/food-beverage/stages/insert
Content-Type: application/json
```

```json
{
  "run": "L01-260810-002",
  "stage": "fill",
  "machine": "EQ010",
  "start": "2026-08-10T22:10:00+02:00",
  "end": "2026-08-11T05:50:00+02:00"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `run` | string | yes | Business code of the production run. |
| `stage` | string | yes | Stage code within the run. |
| `machine` | string | yes | Business code of the machine performing the stage. |
| `start` | string | yes | Date and time when the stage started. |
| `end` | string or null | no | Date and time when the stage ended. |


### Update a stage

`PUT /services/pulse/food-beverage/stages/update`

Updates an existing stage.

The stage is identified by the combination of `run` and `stage`.

#### Request

```http
PUT /services/pulse/food-beverage/stages/update
Content-Type: application/json
```

```json
{
  "run": "L01-260810-002",
  "stage": "fill",
  "machine": "EQ010",
  "start": "2026-08-10T22:10:00+02:00",
  "end": "2026-08-11T06:00:00+02:00"
}
```


### Patch a stage

`PATCH /services/pulse/food-beverage/stages/patch`

Partially updates an existing stage.

The exact PATCH identification shape still needs to be verified against the implementation.

#### Request

```http
PATCH /services/pulse/food-beverage/stages/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "run": "L01-260810-002",
    "stage": "fill",
    "end": "2026-08-11T06:00:00+02:00"
  }
}
```


### Retrieve a stage

`GET /services/pulse/food-beverage/stages/select`

Returns a stage identified by its run and stage code.

#### Request

```http
GET /services/pulse/food-beverage/stages/select?run=L01-260810-002&stage=fill
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `run` | string | yes | Business code of the production run. |
| `stage` | string | yes | Stage code within the run. |


### List stages

`GET /services/pulse/food-beverage/stages/query`

Returns stages matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/stages/query?run=L01-260810-002&machine=EQ010
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `run` | string | no | Limits results to stages belonging to the specified run. |
| `stage` | string | no | Limits results to the specified stage code. |
| `machine` | string | no | Limits results to stages performed by the specified machine. |


### Delete a stage

`DELETE /services/pulse/food-beverage/stages/delete`

Deletes a stage.

#### Request

```http
DELETE /services/pulse/food-beverage/stages/delete?run=L01-260810-002&stage=fill
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `run` | string | yes | Business code of the production run. |
| `stage` | string | yes | Stage code within the run. |