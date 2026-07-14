# Getting started

The Pulse API is a REST API for submitting operational data and retrieving Pulse results.

The API uses resource-oriented endpoints, accepts JSON request bodies, returns JSON responses, and uses standard HTTP methods and status codes.

Authentication is handled with bearer tokens.

## API access

To start an integration, you need:

- the Pulse API base URL,
- a valid bearer token,
- the Pulse OpenAPI specification  (`openapi.json`),
- access to the relevant source data in your organization.

The OpenAPI specification describes the available endpoints, request fields, parameters, authentication requirements, and response schemas.

You can import the specification into [Scalar](https://scalar.com/) to inspect and test individual API requests.

Scalar is intended for API exploration and testing. Production data exchange should be implemented in an integration service, application, script, middleware process, or another automated workflow.

## Authentication

Send the bearer token in the `Authorization` header:

```
Authorization: Bearer <access-token>
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

Reference records should normally be created before records that depend on them.

For example:

- Measure unit → Product

- Plant → Production line → Batch → Stage

## First integration

Start with a small, complete scenario:

1. Authenticate.
2. Test a read-only request.
3. Create the required master data.
4. Submit one operational record.
5. Store the returned Pulse identifiers.
6. Validate the submitted data.
7. Automate the same flow in integration code.

## Next steps

- [Authentication](authentication.md)
- [Send your first request](first-request.md)
- [Map your data](../integration/index.md)
- [Master data](../integration/master-data.md)
- [Operational data](../integration/operational-data.md)
- [Measurements](../integration/measurements.md)
- [API reference](../api/index.md)




