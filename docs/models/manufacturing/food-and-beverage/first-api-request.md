# First API request

Use the [Plant](master-data/plant.md) service for your first test request.

A Plant is a simple master-data record and does not depend on another Pulse entity.

## 1. Open the API reference

Open the Plant service in [Scalar](https://scalar.com/) under:

```text
/services/pulse/types/plants
```

Use the API reference to confirm the available operations and request schema.

## 2. Create a test plant

Submit a Plant with a unique `code`.

```json
{
  "code": "TEST-PLANT-001",
  "name": "Test plant"
}
```

Use a code reserved for test data so the record can be identified easily later.

## 3. Verify the response

Confirm that Pulse returns the created Plant with its assigned `id`.

```json
{
  "id": 123,
  "code": "TEST-PLANT-001",
  "name": "Test plant"
}
```

Use the returned `id` when another record references this Plant.

## 4. Retrieve the plant by code

Retrieve the Plant using its `code` and confirm that the returned record matches the submitted data.

Do not create a permanent source-system mapping between `code` and `id`. Retrieve the record by `code` whenever the current Pulse `id` is required.

## 5. Test an update

Use update to replace the complete editable representation of the Plant.

```json
{
  "code": "TEST-PLANT-001",
  "name": "Updated test plant"
}
```

Use patch when changing only selected supported attributes.

## Next steps

After the Plant request succeeds:

1. Synchronize the remaining required [master data](master-data/index.md).
2. Create a [Batch](manufacturing/batch.md).
3. Create the related [Stages](manufacturing/stage.md).
4. Submit plans, usage, output, downtime, waste, or [measurements](measurements.md).

See [Updates and error handling](../../../integration/updates-and-error-handling.md) for guidance on corrections, retries, and duplicate prevention.