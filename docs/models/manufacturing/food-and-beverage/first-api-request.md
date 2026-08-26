# First API request

Use the [Plant](master-data/site.md) resource for your first test request.

A Plant is a simple master-data record and does not depend on another Pulse resource.

## 1. Open the API reference

Open the Plant resource in [Scalar](https://scalar.com/) under:

```text
/services/pulse/food-beverage/plants
```

Use the API reference to confirm the supported operations and request schema.

## 2. Submit a test plant

Submit a Plant with a unique business `code`.

```json
{
  "code": "TEST-PLANT-001",
  "name": "Test plant"
}
```

Use a code reserved for test data so the record can be identified easily later.

## 3. Verify the response

Confirm that Pulse accepted the record and returned the submitted Plant.

Pulse may include an internal `id` in the response, but Food & Beverage requests do not use that identifier as an input.

The business `code` is the identifier used by integrations.

## 4. Retrieve the plant

Retrieve the Plant by its `code` and confirm that the returned record matches the submitted values.

## 5. Test a correction

Submit the same `code` with a corrected value:

```json
{
  "code": "TEST-PLANT-001",
  "name": "Updated test plant"
}
```

The existing Plant is corrected rather than duplicated.

Fields omitted from partial updates remain unchanged where the operation supports partial updates.

## Next steps

After the Plant request succeeds:

1. Synchronize the remaining required [master data](master-data/index.md).
2. Submit a [Run](production-activities/run.md) or [Batch](production-activities/batch.md), depending on the production process.
3. Submit related [Consumption](operational-data/consumption.md), [Output](manufacturing/output.md), and [Readings](operational-data/reading.md).
4. Add [Line states](manufacturing/line-state.md), [Events](operational-data/event.md), maintenance, quality holds, and complaints when applicable.

See the [API reference](api/index.md) for API conventions, corrections, validation, and supported operations.