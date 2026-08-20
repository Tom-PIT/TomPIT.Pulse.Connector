# Getting started

The Pulse API is a REST API for submitting operational data and working with Pulse integration services.

Each organization accesses Pulse through its own API instance. The base URL is provided during setup.

Pulse supports different operational environments through connector models. A connector model defines how source-system data is represented and submitted for a particular industry or use case.

## API access

To start an integration, you need:

- The API base URL assigned to your organization.
- A valid bearer token.
- Access to the Pulse OpenAPI specification.
- Access to the source data you want to integrate.
- The connector model that applies to your integration.

Do not hard-code an example hostname. Use the base URL provided during setup.

The OpenAPI specification describes the available endpoints, request fields, parameters, authentication requirements, and response schemas.

You can use [Scalar](https://scalar.com/) to inspect the available services, request schemas, and responses in your Pulse instance.

Scalar is intended for API exploration and testing. For automated integrations, production data exchange is typically implemented in an integration service, application, script, middleware process, or another automated workflow.

## Authentication

Send the bearer token in the `Authorization` header:

```http
Authorization: Bearer <your-api-token>
```

See [Authentication](authentication.md) for details.

## Excel integration templates

For supported models, Pulse may provide an Excel integration template that can be completed directly by operational or business users.

The template provides a structured way to prepare master data and operational records before they are submitted to Pulse. It is especially useful when source data is collected manually, exported from existing systems, or prepared by teams that do not work directly with the Pulse API.

Where available, using the model-specific Excel template is the recommended starting point for defining and validating the data required by the integration.

The selected model documentation provides the applicable template and usage instructions.

## Integration flow

A typical integration follows this sequence:

``` mermaid
graph LR
  A[Source systems] --> B[Choose connector model]
  B --> C[Map source data]
  C --> D[Submit data through Pulse API]
  D --> E[Verify submitted data]
```

The exact entities, relationships, and submission order depend on the selected connector model.

## Choose a connector model

Before mapping source data, select the connector model that matches the operational environment you are integrating.

Each connector model provides its own:

- Data model and terminology.
- Entity relationships.
- Submission order.
- Integration examples.
- API guidance.

See [Connector models](../models/index.md).

## Next steps

1. [Configure authentication](authentication.md).
2. [Choose a connector model](../models/index.md).
3. Follow the integration guide for the selected connector model.
4. Use the API reference for the selected connector model.