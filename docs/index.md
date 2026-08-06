# Pulse integration documentation

Pulse uses operational data from source systems to connect business activities, identify patterns, evaluate their impact, and generate recommendations.

Pulse is not limited to manufacturing. It can be used in production, retail, wholesale, logistics, services, and other operational environments where data is exchanged between systems and business activities need to be analyzed.

Pulse uses a shared Tenant data model across industries. Industry-specific connectors map source-system data into this common model, while profiles define how Pulse interprets that data for analysis.

This documentation explains the general integration process, the shared Pulse data model, and the available connector profiles.

```mermaid
graph LR
  A[Source systems] --> B[Industry-specific connector]
  B --> C[Pulse Tenant data model]
  C --> D[Active profile]
  D --> E[Analysis]
  E --> F[Answers]
```

## Start here

1. [Review the integration overview](getting-started/index.md).
2. [Authenticate with the Pulse API](getting-started/authentication.md).
3. [Send your first data](getting-started/first-api-request.md).
4. [Validate the result](getting-started/validation.md).
5. [Map your source data](integration/index.md).

## Reference

- [Pulse data model](data-model/index.md)
- [Entity relationships](data-model/relationships.md)
- [Typical operational day](scenarios/typical-operational-day.md)
- [API reference](api/index.md)
- [Troubleshooting](troubleshooting.md)