# Updates and error handling

Use the operation that matches the intended change.

## Insert, update, and patch

| Operation | Use |
| --- | --- |
| **Insert** | Create a new record. |
| **Update** | Replace the complete editable representation of an existing record. Include all required and editable attributes. |
| **Patch** | Change only the specified attributes of an existing record. Unspecified attributes remain unchanged. |
| **Delete** | Remove an existing record when deletion is supported and appropriate. |

Use the [API reference](../api/index.md) to confirm which operations are available for each service.

## Matching records by code

Pulse matches many records by `code`.

When a matching record already exists, the service may update that record instead of creating a duplicate. Exceptions may apply depending on the entity or service.

Do not rely on repeated insertion as a general update method. Use update or patch when the intended action is to modify an existing record.

When a request requires a Pulse `id`, retrieve the related record by `code` and use the returned `id`.

## Correcting existing records

Retrieve the existing record before updating it when you need its current values or Pulse `id`.

Planned and actual records are separate. Update the plan record when the expected value changes. Update the usage record when correcting what actually occurred.

For shared-identity records, use the same `id` as the related parent record:

- Batch plan and Batch usage use the Batch `id`.
- Stage plan and Stage usage use the Stage `id`.
- Downtime plan and Downtime usage use the Downtime `id`.

## Submission order and dependencies

Submit parent records before records that reference them.

For example:

1. Create or retrieve the required master data.
2. Create the parent record.
3. Submit records that reference the parent.
4. Submit dependent detail records.

If a request fails because a referenced record does not exist, create or retrieve that record before submitting the request again.

## Handling validation errors

Inspect the response status and error body before retrying a request.

Do not automatically retry requests that fail because of:

- Missing required attributes.
- Invalid attribute values.
- Missing referenced records.
- Unsupported enum values.
- Incorrect identifiers.
- Invalid relationships between records.

Correct the request before submitting it again.

## Retrying failed requests

Retry temporary transport or service failures with a delay.

Do not retry every failure automatically. Validation and dependency errors require a corrected request rather than another identical submission.

When the result of an insert request is uncertain, check whether the record already exists before retrying. Use its `code` or another supported lookup field.

Retry behavior and idempotency guarantees may differ by service. Use the API reference for confirmed service-specific behavior.

## Preventing duplicate records

Use stable source-system codes for records that Pulse matches by `code`.

Before retrying an insert after an uncertain response:

1. Retrieve the record by `code`.
2. If it exists, use the returned `id`.
3. If it does not exist, submit the insert again.

Do not generate a new code for the same business record only because the previous response was interrupted or unavailable.

## Logging

Record enough information to investigate failed requests:

- Timestamp.
- Service path and operation.
- Source-system identifier or code.
- Pulse `id`, when available.
- Request payload.
- Response status.
- Response body or error message.
- Retry count.

Do not log credentials, access tokens, or other secrets.