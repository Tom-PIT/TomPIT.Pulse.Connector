# Troubleshooting

Use this guide when a Pulse integration request fails, produces unexpected results, or appears to have been accepted incorrectly.

For request schemas, supported operations, and resource-specific behavior, use the API reference for the selected model.

## Start with the API response

Inspect the HTTP status and response body before retrying a failed request.

Pulse errors may identify:

- the error type;
- the affected field;
- a missing referenced record;
- an unsupported value;
- an invalid relationship;
- another validation problem.

For example:

```json
{
  "error": "unknown-entity",
  "detail": "no line with code 'L07'",
  "path": "line"
}
```

Correct validation or dependency errors before submitting the request again.

## Check the request against the model

When a request is rejected, verify:

- the correct model-specific endpoint is being used;
- required fields are present;
- field names use the documented camelCase spelling;
- values use the documented JSON types;
- enum values are supported;
- referenced business codes exist;
- timestamps use ISO 8601 format with an explicit UTC offset;
- units and quantities are consistent with the resource being submitted.

See [Validation](validation.md) for general validation guidance.

## Check dependencies

Many resources reference records that must already exist.

For example, a production line may require an existing site, and a production run may reference an existing line, product, recipe, shift, or crew.

If the API reports a missing dependency:

1. Identify the referenced business code.
2. Confirm the record exists in the appropriate resource.
3. Submit or correct the referenced record if necessary.
4. Retry the dependent request.

Do not substitute Pulse internal identifiers for model-specific business-code references.

## Corrections and duplicate records

Use stable source-system keys for records submitted to Pulse.

When a request is replayed with the same business key and the same values, supported resources treat the replay as idempotent rather than creating another business record.

When values need to be corrected, use the operation and correction behavior documented for that resource.

Do not generate a new business code only because an earlier request failed or its response was lost.

## Retrying requests

Retry temporary failures such as:

- network interruption;
- connection timeout;
- temporary service unavailability;
- transient server errors.

Use a delay between retries and avoid retrying indefinitely.

Do not automatically retry requests that fail because of:

- missing required fields;
- unsupported values;
- invalid references;
- invalid relationships;
- malformed timestamps;
- other validation errors.

Those requests must be corrected first.

## When the result of a request is uncertain

A network failure can occur after Pulse has already processed the request.

Before assuming the operation failed, check the resource using the business key owned by the source system.

If the record already exists with the intended values, do not create another identifier and submit a duplicate business record.

Where supported, idempotent replay allows the original request to be safely submitted again.

## Use dry-run validation

Where supported, append:

```text
?dryRun=true
```

to validate a write without persisting it.

This is useful when:

- developing a new integration mapping;
- testing a new resource;
- validating a batch of records before import;
- diagnosing validation failures.

The API reference indicates which operations support dry-run validation.

## Partial success in batch requests

Some stream resources accept arrays of records.

When a batch contains both valid and invalid items, inspect the result for each submitted record rather than assuming that the entire batch succeeded or failed.

Correct and resend only the rejected records when appropriate.

## Unexpected data in Pulse

If a request succeeds but the resulting data appears incorrect, check:

- whether the correct business code was used;
- whether the record was associated with the correct run, batch, line, machine, vessel, lot, or other subject;
- whether timestamps use the intended time zone;
- whether quantities were submitted as individual captures or cumulative totals;
- whether the correct metric, state, event type, or reason was used;
- whether a correction unintentionally changed an existing record.

Compare the submitted payload with the model documentation and API response.

## Logging

Record enough information to investigate integration failures.

Useful log fields include:

- timestamp;
- model and endpoint;
- source-system business key;
- request payload or a safe reference to it;
- HTTP status;
- response body or error code;
- retry count;
- correlation or internal response identifier when available.

Do not log:

- bearer tokens;
- passwords;
- secrets;
- other authentication credentials.

## Common problems

| Problem | Check |
| --- | --- |
| `unknown-entity` or missing reference | Confirm the referenced business code exists. |
| Unsupported enum value | Compare the value with the API schema. |
| Invalid timestamp | Use ISO 8601 with an explicit UTC offset. |
| Duplicate-looking records | Confirm the integration is reusing the same stable business key. |
| Request succeeds but data is linked incorrectly | Check the referenced subject codes. |
| Values appear at the wrong time | Check source-system time zone conversion. |
| Batch request partly fails | Inspect the result for each submitted item. |
| Repeated totals appear inflated | Confirm the source is sending individual captures rather than cumulative totals. |
| Retry keeps failing | Stop retrying and inspect the validation error. |

## Still having problems?

Reproduce the issue with the smallest possible request and compare it with the model-specific API reference.

When reporting an integration problem, include:

- the endpoint;
- a sanitized request payload;
- the response status and body;
- the business code involved;
- the approximate request time.

Do not include API tokens or other secrets.