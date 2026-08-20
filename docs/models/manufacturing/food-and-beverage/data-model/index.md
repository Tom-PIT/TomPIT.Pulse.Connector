# Food & Beverage data model

The Food & Beverage connector model defines how operational records are structured and connected within this integration.

Use this section to understand relationships and identifiers that apply across the Food & Beverage integration areas.

The connector maps these records into Pulse.

## Core concepts

### Entity relationships

Food & Beverage connector entities reference related records through integer `id` values.

Some records depend on a parent entity, while others share the same identity as their parent.

See [Relationships](relationships.md) for:

- Parent and dependent records.
- Shared-identity records.
- Resource plan and usage relationships.
- Waste relationships.
- Downtime and maintenance relationships.

### Dimensions

Some Food & Beverage records use `dimension` and `dimensionId` to identify their operational context.

See [Dimension](dimension.md) for the supported contexts and how `dimensionId` is interpreted.

### Ambient value storage

Pulse stores high-volume Ambient value records across internal shards.

This storage model is transparent to API clients.

See [Ambient value sharding](ambient-value-sharding.md).

## Related guidance

For request requirements, data types, identifiers, date formats, and common validation failures, see [Validation](../../../../integration/validation.md).

For submission order and connector workflow, see [Food & Beverage integration](../index.md).