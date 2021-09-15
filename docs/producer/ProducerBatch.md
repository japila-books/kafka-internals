# ProducerBatch

## Creating Instance

`ProducerBatch` takes the following to be created:

* <span id="tp"> `TopicPartition`
* <span id="recordsBuilder"> MemoryRecordsBuilder
* <span id="createdMs"> createdMs
* <span id="isSplitBatch"> `isSplitBatch` flag (default: `false`)

`ProducerBatch` is created when:

* `ProducerBatch` is requested to [createBatchOffAccumulatorForRecord](#createBatchOffAccumulatorForRecord)
* `RecordAccumulator` is requested to [append a record](RecordAccumulator.md#append)

## <span id="tryAppend"> tryAppend

```java
FutureRecordMetadata tryAppend(
  long timestamp,
  byte[] key,
  byte[] value,
  Header[] headers,
  Callback callback,
  long now)
```

`tryAppend`...FIXME

`tryAppend` is used when:

* `RecordAccumulator` is requested to [append a record](RecordAccumulator.md#append)
