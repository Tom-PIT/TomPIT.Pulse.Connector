# First API request

Use the [Site](master-data/site.md) resource for your first test request.

A Site is a simple master-data record and does not depend on another Pulse resource, making it a good way to verify authentication, routing, and the basic request format.

## 1. Open the API reference

Open the Site resource in the API reference under:

```text
/services/pulse/food-beverage/plants
```

Use the API reference to confirm the supported operations and request schema.

## 2. Create a test site

Create a Site with a unique business `code`.

```http
POST /services/pulse/food-beverage/plants/insert
Content-Type: application/json
```

```json
{
  "code": "TEST-SITE-001",
  "name": "Test site"
}
```

Use a code reserved for test data so the record can be identified and removed easily later.

> [!IMPORTANT]
> `code` is the business identifier used throughout the Food & Beverage API.
>
> References between resources use business codes rather than internal Pulse identifiers.

## 3. Verify the response

Confirm that Pulse accepted the record and returned the Site:

```json
{
  "code": "TEST-SITE-001",
  "name": "Test site"
}
```

The same `code` is used later when another resource needs to reference this Site.

For example, a Production line can reference:

```json
{
  "site": "TEST-SITE-001"
}
```

## 4. Retrieve the site

Retrieve the Site by its business code:

```http
GET /services/pulse/food-beverage/plants/select?id=TEST-SITE-001
```

The `id` query parameter contains the Site's business `code`.

Confirm that the returned record matches the values submitted earlier.

## 5. Test an update

Update the existing Site:

```http
PUT /services/pulse/food-beverage/plants/update
Content-Type: application/json
```

```json
{
  "code": "TEST-SITE-001",
  "name": "Updated test site"
}
```

Retrieve the Site again and confirm that the new name is returned.

## 6. Test a partial update

The Site resource also supports PATCH.

```http
PATCH /services/pulse/food-beverage/plants/patch
Content-Type: application/json
```

```json
{
  "properties": {
    "code": "TEST-SITE-001",
    "name": "Patched test site"
  }
}
```

The Site is identified by `properties.code`.

For Site PATCH requests, both `code` and `name` are required by the current API.

## 7. Delete the test record

After testing, remove the temporary Site:

```http
DELETE /services/pulse/food-beverage/plants/delete?id=TEST-SITE-001
```

Use delete only for records that should no longer exist.

## Next steps

After the first request succeeds, a typical integration proceeds by synchronizing foundational data before submitting operational records.

A practical sequence is:

1. Synchronize the required [Master data](master-data/index.md), such as Sites, Production lines, Machines, Products, Materials, Suppliers, Shifts, and Crews.
2. Register the required [Definitions and rules](definitions-and-rules/index.md), such as Measurements, Product limits, Targets, Reasons, Clean regimes, and Types.
3. Submit [Production activities](production-activities/index.md), such as Lots, Batches, Runs, Planned use, Stages, Cleans, and Holds.
4. Submit [Operational data](operational-data/index.md), including Consumption, Output, Readings, Settings, Line time, and Events.
5. Add [Maintenance and quality](maintenance-and-quality/index.md) data, including Work orders, Parts and Labor, and Complaints.

For a complete example of how these resources fit together, see the [End-to-end integration example](end-to-end-integration.md).

See the [API reference](api/index.md) for supported resources, operations, query parameters, and request conventions.