# Plant

Represents a physical operating location in Pulse.

## The Plant object

```json
{
  "code": "PLANT-LJ",
  "name": "Ljubljana plant",
  "types": {
    "region": "CENTRAL-EUROPE"
  },
  "attributes": {
    "erpCode": "1000"
  }
}
```

## Fields

<div class="attributes-table" markdown>

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `code` | string | Business code used to identify the plant in external systems and integrations. | `"PLANT-LJ"` |
| `name` | string | Human-readable name of the plant. | `"Ljubljana plant"` |
| `types` | object or null | Optional classifications used to group and analyse the plant. | `{ "region": "CENTRAL-EUROPE" }` |
| `attributes` | object or null | Optional additional source-system attributes associated with the plant. These values are stored with the plant but are not used for analysis. | `{ "erpCode": "1000" }` |

</div>

See [Types and attributes](types-and-attributes.md) for guidance on extensible master-data properties.

## API resource

| Resource | Base path |
| --- | --- |
| `Plant` | `/services/pulse/food-beverage/plants` |

See the [API reference](../api/index.md) for supported operations and complete request schemas.

## API methods

### Create or update a plant

`POST /services/pulse/food-beverage/plants`

Creates a plant when the supplied `code` does not already exist.

When a plant with the same `code` already exists, the submitted values update the existing plant. Fields omitted from the request remain unchanged.

#### Request

```http
POST /services/pulse/food-beverage/plants
Content-Type: application/json
```

```json
{
  "code": "PLANT-LJ",
  "name": "Ljubljana plant",
  "types": {
    "region": "CENTRAL-EUROPE"
  },
  "attributes": {
    "erpCode": "1000"
  }
}
```

#### Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `code` | string | yes | Business identifier of the plant. |
| `name` | string | no | Display name of the plant. |
| `types` | object | no | Declared classifications associated with the plant. |
| `attributes` | object | no | Free-form source-system metadata. |

#### Dry run

Add `dryRun=true` to validate the same request without persisting changes.

```http
POST /services/pulse/food-beverage/plants?dryRun=true
Content-Type: application/json
```

```json
{
  "code": "PLANT-LJ",
  "name": "Ljubljana plant",
  "types": {
    "region": "CENTRAL-EUROPE"
  }
}
```

Use dry run when validating mappings or testing a payload before writing it to Pulse.


### List plants

`GET /services/pulse/food-beverage/plants`

Returns the plants available to the integration.

#### Request

```http
GET /services/pulse/food-beverage/plants
```

#### Example response

```json
[
  {
    "code": "PLANT-LJ",
    "name": "Ljubljana plant",
    "types": {
      "region": "CENTRAL-EUROPE"
    },
    "attributes": {
      "erpCode": "1000"
    }
  },
  {
    "code": "PLANT-MB",
    "name": "Maribor plant",
    "types": {
      "region": "CENTRAL-EUROPE"
    }
  }
]
```


### Delete a plant

`DELETE /services/pulse/food-beverage/plants/{code}`

Retracts the plant identified by its business code.

#### Path parameters

| Parameter | Type | Description |
| --- | --- | --- |
| `code` | string | Business code of the plant to retract. |

#### Request

```http
DELETE /services/pulse/food-beverage/plants/PLANT-LJ
```