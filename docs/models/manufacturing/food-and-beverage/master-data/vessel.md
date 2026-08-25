# Vessel

Represents a tank, silo, or other process vessel associated with a production line.

A vessel belongs to a production line and can act as a carryover scope between production batches.

## The Vessel object

```json
{
  "code": "TANK-3",
  "name": "Fermentation tank 3",
  "parent": "LINE001"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Unique business code used to identify the vessel in external systems and integrations. | `"TANK-3"` |
| `name` | string | Human-readable name of the vessel. | `"Fermentation tank 3"` |
| [`parent`](production-line.md) | string | Business code of the production line the vessel belongs to or feeds. | `"LINE001"` |

</div>

> [!IMPORTANT]
> `code` must be unique. Two vessels cannot use the same code.
>
> The production line referenced by `parent` must already exist before the vessel is submitted.

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## API resource

| Resource | Base path |
| --- | --- |
| `Vessel` | `/services/pulse/food-beverage/vessels` |

## API methods

### Create a vessel

`POST /services/pulse/food-beverage/vessels/insert`

Creates a new vessel.

#### Request

```http
POST /services/pulse/food-beverage/vessels/insert
Content-Type: application/json
```

```json
{
  "code": "TANK-3",
  "name": "Fermentation tank 3",
  "parent": "LINE001"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the vessel. |
| `name` | string | yes | Human-readable name of the vessel. |
| `parent` | string | yes | Business code of the production line the vessel belongs to or feeds. |


### Update a vessel

`PUT /services/pulse/food-beverage/vessels/update`

Updates an existing vessel.

#### Request

```http
PUT /services/pulse/food-beverage/vessels/update
Content-Type: application/json
```

```json
{
  "code": "TANK-3",
  "name": "Fermentation tank 3",
  "parent": "LINE002"
}
```

Changing `parent` moves the vessel to another production line.

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Unique business code of the vessel to update. |
| `name` | string | yes | Human-readable name of the vessel. |
| `parent` | string | yes | Business code of the production line the vessel belongs to or feeds. |


### Patch a vessel

`PATCH /services/pulse/food-beverage/vessels/patch`

Partially updates an existing vessel.

The fields to update are supplied in the `properties` object. The vessel is identified by its business `code`.

#### Request

```http
PATCH /services/pulse/food-beverage/vessels/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "TANK-3",
    "name": "Fermentation tank 3",
    "parent": "LINE002"
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Unique business code of the vessel to update. |
| `properties.name` | string | no | New human-readable name of the vessel. |
| `properties.parent` | string | no | Business code of the production line the vessel should belong to. |


### Retrieve a vessel

`GET /services/pulse/food-beverage/vessels/select`

Returns the vessel identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/vessels/select?id=TANK-3
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the vessel to retrieve. |

#### Example response

```json
{
  "code": "TANK-3",
  "name": "Fermentation tank 3",
  "parent": "LINE001"
}
```


### List vessels

`GET /services/pulse/food-beverage/vessels/query`

Returns vessels matching the supplied filters.

Vessels can be filtered by code, name, and parent production line.

#### Request

```http
GET /services/pulse/food-beverage/vessels/query?parents=LINE001
```

#### Query parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `codes` | string or array of strings | no | Limits results to vessels with the specified business codes. |
| `names` | string or array of strings | no | Limits results to vessels with the specified names. |
| `parents` | string or array of strings | no | Limits results to vessels belonging to the specified production lines. |

#### Example response

```json
[
  {
    "code": "TANK-3",
    "name": "Fermentation tank 3",
    "parent": "LINE001"
  },
  {
    "code": "TANK-4",
    "name": "Fermentation tank 4",
    "parent": "LINE001"
  }
]
```


### Delete a vessel

`DELETE /services/pulse/food-beverage/vessels/delete`

Deletes the vessel identified by its business code.

#### Request

```http
DELETE /services/pulse/food-beverage/vessels/delete?id=TANK-3
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | Business code of the vessel to delete. |