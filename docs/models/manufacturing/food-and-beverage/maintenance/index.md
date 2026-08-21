# Maintenance

Maintenance data describes preventive and corrective work performed on machines in Food & Beverage operations.

The [Maintenance](maintenance.md) resource records the maintenance activity itself, including its planned and actual timing.

Actual materials, labor, equipment, energy, and other resources used during maintenance are submitted through [Consumption](../manufacturing/consumption.md).

When maintenance affects production-line availability, record the corresponding [Line state](../manufacturing/line-state.md) separately.

## Available maintenance data

| Resource | Purpose |
| --- | --- |
| [**Maintenance**](maintenance.md) | Records preventive or corrective maintenance work performed on a machine. |
| [**Consumption**](../manufacturing/consumption.md) | Records resources actually consumed by maintenance work. |

See the [API reference](../api/index.md#maintenance) for supported operations and endpoint paths.