# Pulse integration documentation

Pulse uses operational data from source systems to connect business activities, identify patterns, evaluate their impact, and generate recommendations.

Pulse is designed for different industries and operational environments, including production, retail, wholesale, logistics, services, and others.

Integrations use industry-specific **models** to map source-system data into Pulse. Each model defines the entities, relationships, and integration flow that apply to a particular operational context.

Pulse uses a shared internal data model to process this information consistently across industries. The internal model itself is not part of this integration documentation.

```mermaid
graph LR
  A[Source systems] --> B[Model]
  B --> C[Pulse]
  C --> D[Intelligence]
  D --> E[Answers]
```

## Model assignment

Each Pulse tenant uses one model, and that model cannot be changed after the tenant is created.

A single legal company may therefore use multiple Pulse tenants when different parts of the organization require different models or must be integrated separately.

For example:

- A company that operates multiple distinct business units, such as a retail business and a manufacturing business, may use a separate tenant for each business unit, even though both belong to the same company.
- A company with operations in two countries may use a separate tenant for each country when those operations are managed or integrated separately.

Choose the model that matches the operational context represented by the tenant.

## Start here

1. [Review the integration overview](getting-started/index.md).
2. [Authenticate with the Pulse API](getting-started/authentication.md).
3. [Choose a model](models/index.md), according to your industry or operational environment.
4. Follow the integration guide for that model.

## Models

Models describe how data from a specific industry or operational environment is represented when integrating with Pulse.

Available models:

- [Food & Beverage](models/manufacturing/food-and-beverage/index.md)
- [Metal & Machining](models/manufacturing/metal-and-machining/index.md)

## Reference

- [Troubleshooting](troubleshooting.md)
- [Validation](validation.md)
