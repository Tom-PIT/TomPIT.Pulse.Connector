# Stage

Represents a defined execution step within a production [Run](run.md).

A Stage records when a specific part of the Run was active and which Machine performed that step.

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
| [`run`](run.md) | string | Business code of the production Run to which the Stage belongs. | `"L01-260810-002"` |
| `stage` | string | Stage code that identifies the production step within the Run. | `"fill"` |
| [`machine`](../master-data/machine.md) | string | Business code of the Machine performing the Stage. | `"EQ010"` |
| `start` | string | Date and time when the Stage started, in ISO 8601 format. | `"2026-08-10T22:10:00+02:00"` |
| `end` | string or null | Date and time when the Stage ended, or `null` while it remains open. | `"2026-08-11T05:50:00+02:00"` |

</div>

> [!IMPORTANT]
> `run` and `stage` together identify the Stage.
>
> `run` and `machine` must reference existing records.

## Stage identity

A Stage is identified within its Run by the combination of `run` and `stage`.

For example:

```text
L01-260810-002/fill
```

and:

```text
L01-260810-003/fill
```

represent different Stage executions even though both use the `fill` Stage code.

## Stage status

A Stage without `end` remains open.

When `end` is supplied, the Stage is treated as completed.

```text
end = null    → running
end supplied  → completed
```

## Stages and machines

The Machine is associated with the Stage during which it operates rather than with the entire Run.

For example:

```text
Run
├── fill   → EQ010
├── cap    → EQ020
├── label  → EQ030
└── case   → EQ040
```

This allows Pulse to preserve the actual operating period of each Machine.

A production Run may last eight hours while a particular Machine operates for only part of that time. Recording the Machine on the Stage preserves that distinction.

## Reference protection

A Run or Machine referenced by an existing Stage cannot be deleted until the Stage reference is removed.

## API resource

| Resource | Base path |
| --- | --- |
| `Stage` | `/services/pulse/food-beverage/stages` |

## API methods

### Create a stage

`POST /services/pulse/food-beverage/stages/insert`

Creates a Stage within a production Run.

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
| `run` | string | yes | Business code of the production Run. |
| `stage` | string | yes | Stage code within the Run. |
| `machine` | string | yes | Business code of the Machine performing the Stage. |
| `start` | string | yes | Date and time when the Stage started. |
| `end` | string or null | no | Date and time when the Stage ended. |

### Update a stage

`PUT /services/pulse/food-beverage/stages/update`

Updates an existing Stage.

The Stage is identified by the combination of `run` and `stage`.

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

Partially updates an existing Stage.

The Stage is identified by `properties.run` and `properties.stage`. Fields omitted from `properties` keep their current values.

`end` can be explicitly cleared by including it with a null value.

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

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.run` | string | yes | Business code of the Run containing the Stage. |
| `properties.stage` | string | yes | Stage code identifying the Stage within the Run. |
| `properties.machine` | string | no | New Machine business code. |
| `properties.start` | string | no | New Stage start date and time. |
| `properties.end` | string or null | no | New Stage end date and time, or `null` to leave the Stage open. |

### Retrieve a stage

`GET /services/pulse/food-beverage/stages/select`

Returns the Stage identified by its Run and Stage code.

#### Request

```http
GET /services/pulse/food-beverage/stages/select?run=L01-260810-002&stage=fill
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `run` | string | yes | Business code of the production Run. |
| `stage` | string | yes | Stage code within the Run. |

### List stages

`GET /services/pulse/food-beverage/stages/query`

Returns Stages matching the supplied filters.

#### Request

```http
GET /services/pulse/food-beverage/stages/query?runs=L01-260810-002&machines=EQ010
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `runs` | string or array of strings | no | Limits results to Stages belonging to the specified Runs. |
| `stages` | string or array of strings | no | Limits results to the specified Stage codes. |
| `machines` | string or array of strings | no | Limits results to Stages performed by the specified Machines. |
| `from` | string | no | Limits results to Stages starting at or after the specified date and time. |
| `to` | string | no | Limits results to Stages starting at or before the specified date and time. |

Multiple values can be supplied by repeating the query parameter:

```http
GET /services/pulse/food-beverage/stages/query?stages=fill&stages=cap
```

### Delete a stage

`DELETE /services/pulse/food-beverage/stages/delete`

Deletes the Stage identified by its Run and Stage code.

#### Request

```http
DELETE /services/pulse/food-beverage/stages/delete?run=L01-260810-002&stage=fill
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `run` | string | yes | Business code of the production Run. |
| `stage` | string | yes | Stage code within the Run. |