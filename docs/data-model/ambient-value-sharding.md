# Ambient value sharding

Pulse uses sharding to manage large volumes of Ambient value records efficiently.

Ambient values can arrive frequently from sensors, meters, equipment, production lines, rooms, and other sources. Sharding allows Pulse to distribute these records across manageable storage partitions without changing their business meaning.

## How sharding works

Ambient value shards are organized by month.

Pulse does not automatically create a new shard at the beginning of every month. A new time-based shard is used only after the current shard exceeds 100,000 records.

If the current shard contains 100,000 records or fewer, new Ambient value records continue to be stored in that shard, even when they belong to a later month.

Once the current shard exceeds 100,000 records, Pulse uses the next time-based shard for subsequent records.

## Example

| Record period | Total records in the current shard | Result |
| --- | ---: | --- |
| January | 42,000 | The current shard remains open. |
| February | 78,000 | February records are added to the current shard. |
| March | 118,000 | The current shard exceeds the threshold. |
| April | New shard | Subsequent records are stored in the next shard. |

## Integration behavior

Sharding is handled internally by Pulse.

Integrations do not need to:

- Select a shard.
- Submit a shard identifier.
- Track where a record is stored.
- Query individual shards directly.

Ambient value records should be submitted and queried through the normal API operations. Pulse handles the underlying shard selection transparently.

## Relationship to Ambient value

An [Ambient value](../integration/manufacturing/ambient-value.md) represents an individual measured or received value.

Ambient value sharding defines how large volumes of those records are organized for efficient storage and retrieval. It does not alter the value, timestamp, type, dimension, or other business data contained in the record.