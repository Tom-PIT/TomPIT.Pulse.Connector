# Measurements

Pulse currently represents measured or received operational values as [Ambient value](manufacturing/ambient-value.md) records.

Before submitting measurements, create or retrieve the related:

- [Ambient type](master-data/ambient-type.md), which defines what is measured and its measure unit.
- [Dimension](data-model/dimension.md), which defines the operational context.
- Context record identified by `dimensionId`.

Each Ambient value records:

- The measurement timestamp.
- The measured value.
- The measurement type.
- The operational context.
- Optional minimum, maximum, and expected values.

Use the most precise context available from the source system. A value linked to a production line, stage, equipment record, or batch provides more specific operational context than a value linked only to a plant.

For high-volume measurements, Pulse handles storage through [Ambient value sharding](data-model/ambient-value-sharding.md). Integrations do not select or manage shards.

See [Ambient value](manufacturing/ambient-value.md) for the object fields and API service.