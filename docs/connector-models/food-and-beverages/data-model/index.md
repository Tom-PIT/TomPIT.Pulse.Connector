# Serial Production data model

The Serial Production connector model defines how operational records are structured and connected for this connector profile.

Use this section to understand relationships and identifiers that apply across the Serial Production integration areas.

The connector maps these records into the shared Pulse Tenant data model.

## Core concepts

### Entity relationships

Serial Production connector entities reference related records through integer `id` values.

Some records depend on a parent entity, while others share the same identity as their parent.

See [Relationships](relationships.md) for:

- Parent and dependent records.
- Shared-identity records.
- Resource plan and usage relationships.
- Waste relationships.
- Downtime and maintenance relationships.

### Dimensions

Some Serial Production records use `dimension` and `dimensionId` to identify their operational context.

See [Dimension](dimension.md) for the supported contexts and how `dimensionId` is interpreted.

### Ambient value storage

Pulse stores high-volume Ambient value records across internal shards.

This storage model is transparent to API clients.

See [Ambient value sharding](ambient-value-sharding.md).

## Related guidance

For request requirements, data types, identifiers, date formats, and common validation failures, see [Validation](../validation.md).

For submission order and connector workflow, see [Serial Production integration](../index.md).