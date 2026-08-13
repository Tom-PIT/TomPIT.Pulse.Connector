# Validation

Pulse validates each request before accepting it.

Use the entity documentation and API reference to confirm the required attributes and supported values for each operation.

## Required attributes

Include every attribute required by the selected operation.

Do not submit empty values unless the schema explicitly allows them.

## Data types and formats

Use the documented JSON type for each attribute.

Common types include:

- `integer` for Pulse identifiers.
- `number` for quantities, prices, percentages, and measured values.
- `string` for codes, names, and other text values.
- ISO 8601 strings for date and time values.
- `boolean` for true or false values.

Use the documented camelCase property names.

## Identifiers and references

Referenced records must exist before they are used in another request.

When a request requires a Pulse `id`:

1. Retrieve the related record by `code`.
2. Use the returned `id`.
3. Submit the dependent record.

Do not create a permanent source-system mapping between `code` and `id`.

For shared-identity records, use the same `id` as the related parent record:

- Batch plan and Batch usage use the Batch `id`.
- Stage plan and Stage usage use the Stage `id`.
- Downtime plan and Downtime usage use the Downtime `id`.

## Codes

Use a stable source-system code that identifies the record within the applicable Pulse resource.

Avoid generating a new code for the same business record during retries or corrections.

## Enum values

Enum attributes accept only supported values.

Use the relevant entity page or API reference to confirm the available values.

Examples include:

- [Dimension](data-model/dimension.md).
- Maintenance kind.
- Other entity-specific classifications documented in the API.

## Dates and time ranges

Use valid ISO 8601 date and time values.

Keep time zones consistent across related records.

When a record contains a start and end time, ensure the end does not precede the start.

## Numeric values

Use the unit expected by the related entity or master-data record.

Examples:

- Material and energy quantities use the configured measure unit.
- Time-based prices are expressed per hour.
- Percentages are submitted as decimal fractions.

For example, submit 25% as:

```json
{
  "percentage": 0.25
}
```

Do not submit `25` for a value that represents 25%.

## Relationship rules

Some records are valid only in a specific relationship.

Examples:

- A Stage belongs to a Batch.
- Resource plans and usage records belong to a Stage.
- Waste detail records belong to a Waste record.
- Downtime maintenance requires both a Downtime and Maintenance record.
- `dimensionId` must identify a record that matches the selected [Dimension](data-model/dimension.md).

Create or retrieve the required parent records before submitting dependent records.

## Validation workflow

Before submitting a request:

1. Validate the JSON structure locally.
2. Confirm required master data exists.
3. Retrieve referenced records by `code`.
4. Use the returned Pulse `id` values.
5. Confirm enum values.
6. Confirm date, time, and numeric formats.
7. Submit parent records before dependent records.
8. Inspect the response before continuing.

## Common validation failures

| Failure | Check |
| --- | --- |
| Missing required attribute | Compare the payload with the entity schema. |
| Invalid identifier | Retrieve the referenced record and use its current Pulse `id`. |
| Missing dependency | Create or retrieve the parent record first. |
| Unsupported enum value | Use one of the documented values. |
| Invalid date range | Confirm the format, time zone, and chronological order. |
| Invalid percentage | Submit the value as a decimal fraction. |
| Incorrect property name | Use the documented camelCase field name. |
| Incorrect data type | Use the JSON type defined by the schema. |

See [Updates and error handling](../../integration/updates-and-error-handling.md) for guidance on correcting requests, retrying failures, and preventing duplicate records.