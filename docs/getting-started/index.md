# Getting started

The Pulse API is a REST API for submitting operational data and retrieving Pulse results.

Each organization accesses Pulse through its own API instance. The base URL is provided during setup.

The API uses resource-oriented endpoints, accepts JSON request bodies, returns JSON responses, and uses bearer-token authentication.

## API access

To start an integration, you need:

- The API base URL assigned to your organization
- A valid bearer token
- The Pulse OpenAPI specification (`openapi.json`)
- Access to the relevant source data in your organization

Each organization accesses Pulse through its own API instance. Do not hard-code an example hostname; use the base URL provided during setup.

The OpenAPI specification describes the available endpoints, request fields, parameters, authentication requirements, and response schemas.

You can use [Scalar]https://scalar.com/) to inspect the available services, request schemas, and responses in your Pulse instance..

Scalar is intended for API exploration and testing. Production data exchange should be implemented in an integration service, application, script, middleware process, or another automated workflow.

## Authentication

Send the bearer token in the `Authorization` header:

```
Authorization: Bearer <your-api-token>
```

See [Authentication](authentication.md) for details.

## Data flow

A typical integration follows this sequence:

``` mermaid
graph LR
  A[Source data] --> B[Data mapping]
  B --> C[Master data]
  C --> D[Operational data]
  D --> E[Measurements]
  E --> F[Verify submitted data]
```

[Master-data records](../integration/master-data/index.md) should normally be created before records that depend on them.

For example:

- Measure unit → Product
- Plant → Production line → Batch → Stage

## First integration

Start with a small, complete scenario:

1. Configure the organization-specific base URL.
2. Generate and securely store an API token.
3. Send a test request using the [Plant](../integration/master-data/plant.md) service.
4. Retrieve the Plant by its `code`.
5. Create the remaining required master data.
6. Submit one operational record.
7. Validate the submitted data.
8. Automate the same flow in integration code.

## Next steps

- [Authentication](authentication.md)
- [Send your first request](first-api-request.md)
- [Map your data](../integration/index.md)
   - [Master data](../integration/master-data/index.md)
   - [Manufacturing data](../integration/manufacturing/index.md)
   - [Maintenance data](../integration/maintenance/index.md)
- [Validation](../getting-started/validation.md)
- [Updates and error handling](../integration/updates-and-error-handling.md)
- [Measurements](../integration/measurements.md)
- [API reference](../api/index.md)