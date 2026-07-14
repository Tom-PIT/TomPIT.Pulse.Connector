# Pulse Integration Documentation

Pulse uses operational data from source systems to connect business activities, identify patterns, evaluate their impact, and generate recommendations.

This documentation explains how to map source data to the Pulse data model and send it through the Pulse API.

``` mermaid
graph LR
  A[Source systems]
  --> B[Data mapping] 
  --> C[Pulse API] 
  --> D[Pulse data model]
  --> E[Analyses and recommendations];
```

Pulse can receive data from ERP, MES, SCADA, IoT, maintenance, quality, logistics, supply-chain, and other operational systems.

## Start here

1. [Review the integration overview](getting-started/index.md).
2. [Authenticate with the Pulse API](getting-started/authentication.md).
3. [Send your first data](getting-started/first-request.md).
4. [Validate the result](getting-started/validation.md).
5. [Map your source data](integration/index.md).

## Reference

- [Pulse data model](data-model/index.md)
- [Entity relationships](data-model/relationships.md)
- [Typical operational day](scenarios/typical-operational-day.md)
- [API reference](api/index.md)
- [Troubleshooting](troubleshooting.md)