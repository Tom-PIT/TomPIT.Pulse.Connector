# Validation

Pulse validates each request before accepting it.

Use the documentation for the selected model and the API reference to confirm required fields, supported values, relationships, and operation-specific rules.

## Required fields

Include all fields required by the selected operation.

Do not submit empty or `null` values unless the schema explicitly allows them.

For partial updates, omitted fields remain unchanged where supported.

## Data types

Use the documented JSON type for each field.

Common types include:

- `string` for business codes, names, timestamps, and other text values;
- `number` for quantities, values, prices, and measurements;
- `boolean` for true or false values;
- arrays and objects where defined by the resource schema.

Use the documented camelCase field names.

## Business codes and references

Resources use documented business identifiers to identify records and relationships.

For example:

```json
{
  "line": "L03",
  "product": "SKU-4471"
}
```

Referenced records must exist before they are used by another request.

Use the documented business codes for resource references.

## Enum values

Fields with a defined set of values accept only the supported values.

For example, a resource may define allowed values for:

- status;
- maintenance work order;
- output kind;
- line time;
- event type;
- disposition.

Use the resource documentation or API reference to confirm the available values.

## Timestamps

Use ISO 8601 timestamps with an explicit UTC offset where timestamps are required.

For example:

```text
2026-08-10T22:00:00+02:00
```

When a record contains a start and end timestamp:

- both timestamps must use valid ISO 8601 syntax;
- the end must not precede the start;
- related records should use consistent time-zone handling.

## Quantities, units, and values

Use the unit expected by the resource being submitted.

Keep related values consistent. For example:

```json
{
  "quantity": 940,
  "unit": "kg"
}
```

Do not convert or reinterpret values unless required by the model or source-system mapping.

## Relationships

Some resources depend on records that must already exist.

For example, in the Food & Beverage model:

- a production line references a site;
- a run references a production line and product;
- a batch references a vessel and recipe;
- a reading references a measurement and exactly one subject..

Resource pages document their specific dependencies.

Submit referenced records before records that depend on them.

## Dry-run validation

Where supported, append:

```text
?dryRun=true
```

to validate a write without persisting it.

This is useful when developing mappings, testing new payloads, or validating records before import.

The API reference indicates which operations support dry-run validation.

## Validation workflow

Before submitting data:

1. Validate the JSON structure.
2. Confirm required fields are present.
3. Confirm field names and JSON types.
4. Confirm referenced records exist.
5. Confirm enum values.
6. Confirm timestamp formats and ranges.
7. Confirm quantity and unit consistency.
8. Submit referenced records before dependent records.
9. Inspect the API response before continuing.

## Common validation issues

| Problem | Check |
| --- | --- |
| Required data is missing | Compare the payload with the resource schema. |
| A referenced record cannot be resolved | Confirm the referenced business code exists. |
| A value is not accepted | Check the allowed values in the API schema. |
| A timestamp is rejected | Confirm ISO 8601 format and an explicit UTC offset. |
| A relationship is rejected | Check the resource's dependency and subject rules. |

See [Troubleshooting](troubleshooting.md) for guidance on diagnosing failed requests, retries, and unexpected results.