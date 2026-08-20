# Pulse integration documentation

Pulse uses operational data from source systems to connect business activities, identify patterns, evaluate their impact, and generate recommendations.

Pulse is designed for different industries and operational environments, including production, retail, wholesale, logistics, services, and others.

Integrations use industry-specific **connector models** to map source-system data into Pulse. Each connector model defines the entities, relationships, and integration flow that apply to a particular operational context.

Pulse uses a shared internal data model to process this information consistently across industries. The internal model itself is not part of this integration documentation.

```mermaid
graph LR
  A[Source systems] --> B[Connector model]
  B --> C[Pulse]
  C --> D[Analysis]
  D --> E[Answers]
```

## Start here

1. [Review the integration overview](getting-started/index.md).
2. [Authenticate with the Pulse API](getting-started/authentication.md).
3. [Choose a model](models/index.md), according to your industry or operational environment.
4. Follow the integration guide for that model.

## Models

Models describe how data from a specific industry or operational environment is represented when integrating with Pulse.

Available models:

- [Food & Beverages](models/manufacturing/food-and-beverage/index.md)

## Reference

- [Troubleshooting](troubleshooting.md)
