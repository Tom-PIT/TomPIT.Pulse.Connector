# Vessel

Represents a tank, silo, or other process vessel associated with a production line.

A vessel belongs to a production line and can act as a carryover scope between production batches.

## The Vessel object

```json
{
  "code": "TANK-3",
  "name": "Fermentation tank 3",
  "line": "LINE001"
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the vessel in external systems and integrations. | `"TANK-3"` |
| `name` | string | Human-readable name of the vessel. | `"Fermentation tank 3"` |
| [`line`](production-line.md) | string | Business code of the production line the vessel belongs to or feeds. | `"LINE001"` |

</div>

> [!IMPORTANT]
> The production line referenced by [`line`](production-line.md) must already exist before the vessel is submitted.

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## API resource

| Resource | Base path |
| --- | --- |
| `Vessel` | `/services/pulse/food-beverage/vessels` |

## API methods

### Create a vessel

`POST /services/pulse/food-beverage/vessels`

Creates a new vessel.

#### Request

```http
POST /services/pulse/food-beverage/vessels
Content-Type: application/json
```

```json
{
  "code": "TANK-3",
  "name": "Fermentation tank 3",
  "line": "LINE001"
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Business code of the vessel. |
| `name` | string | yes | Human-readable name of the vessel. |
| `line` | string | yes | Business code of the production line the vessel belongs to or feeds. |


### Update a vessel

`PUT /services/pulse/food-beverage/vessels`

Updates an existing vessel.

#### Request

```http
PUT /services/pulse/food-beverage/vessels
Content-Type: application/json
```

```json
{
  "code": "TANK-3",
  "name": "Fermentation tank 3",
  "line": "LINE002"
}
```

Changing `line` moves the vessel to another production line.

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Business code of the vessel to update. |
| `name` | string | yes | Human-readable name of the vessel. |
| `line` | string | yes | Business code of the production line the vessel belongs to or feeds. |


### Patch a vessel

`PATCH /services/pulse/food-beverage/vessels`

Partially updates an existing vessel.

The fields to update are supplied in the `properties` object. The vessel is identified by its business `code`.

#### Request

```http
PATCH /services/pulse/food-beverage/vessels
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "TANK-3",
    "name": "Fermentation tank 3",
    "line": "LINE002"
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `properties` | object | yes | Fields included in the partial update. |
| `properties.code` | string | yes | Business code of the vessel to update. |
| `properties.name` | string | no | New human-readable name of the vessel. |
| `properties.line` | string | no | New production line the vessel belongs to or feeds. |


### Retrieve a vessel

`GET /services/pulse/food-beverage/vessels/{code}`

Returns the vessel identified by its business code.

#### Request

```http
GET /services/pulse/food-beverage/vessels/TANK-3
```

#### Example response

```json
{
  "code": "TANK-3",
  "name": "Fermentation tank 3",
  "line": "LINE001"
}
```


### List vessels

`GET /services/pulse/food-beverage/vessels`

Returns vessels matching the supplied filters.

Vessels can be filtered by production line.

#### Request

```http
GET /services/pulse/food-beverage/vessels?line=LINE001
```

#### Query parameters

| Parameter | Type | Description |
| --- | --- | --- |
| `line` | string | Limits results to vessels belonging to the specified production line. |

#### Example response

```json
[
  {
    "code": "TANK-3",
    "name": "Fermentation tank 3",
    "line": "LINE001"
  },
  {
    "code": "TANK-4",
    "name": "Fermentation tank 4",
    "line": "LINE001"
  }
]
```


### Delete a vessel

`DELETE /services/pulse/food-beverage/vessels/{code}`

Deletes the vessel identified by its business code.

#### Request

```http
DELETE /services/pulse/food-beverage/vessels/TANK-3
```