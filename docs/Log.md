# Log

## Creating Instance

`Log` takes the following to be created:

* <span id="_dir"> Directory
* <span id="config"> `LogConfig`
* <span id="segments"> `LogSegments`
* <span id="logStartOffset"> logStartOffset
* <span id="recoveryPoint"> recoveryPoint
* <span id="nextOffsetMetadata"> `LogOffsetMetadata`
* <span id="scheduler"> `Scheduler`
* <span id="brokerTopicStats"> `BrokerTopicStats`
* <span id="time"> `Time`
* <span id="producerIdExpirationCheckIntervalMs"> producerIdExpirationCheckIntervalMs
* <span id="topicPartition"> `TopicPartition`
* <span id="leaderEpochCache"> Optional `LeaderEpochFileCache`
* <span id="producerStateManager"> `ProducerStateManager`
* <span id="logDirFailureChannel"> `LogDirFailureChannel`
* <span id="_topicId"> Optional Topic ID
* <span id="keepPartitionMetadataFile"> keepPartitionMetadataFile

`Log` is created using [apply](#apply) utility.

## <span id="apply"> Creating Log

```scala
apply(
  dir: File,
  config: LogConfig,
  logStartOffset: Long,
  recoveryPoint: Long,
  scheduler: Scheduler,
  brokerTopicStats: BrokerTopicStats,
  time: Time = Time.SYSTEM,
  maxProducerIdExpirationMs: Int,
  producerIdExpirationCheckIntervalMs: Int,
  logDirFailureChannel: LogDirFailureChannel,
  lastShutdownClean: Boolean = true,
  topicId: Option[Uuid],
  keepPartitionMetadataFile: Boolean): Log
```

`apply`...FIXME

`apply` is used when:

* `LogManager` is requested to `loadLog` and `getOrCreateLog`
* `KafkaMetadataLog` is requested to `apply`

## Reading Messages { #read }

```scala
read(
  startOffset: Long,
  maxLength: Int,
  isolation: FetchIsolation,
  minOneMessage: Boolean): FetchDataInfo
```

`read` prints out the following TRACE message to the logs:

```text
Reading maximum [maxLength] bytes at offset [startOffset] from log with total length [size] bytes
```

`read`...FIXME

`read` requests the `LogSegment` to [read messages](log/LogSegment.md#read).

`read`...FIXME

---

`read` is used when:

* `Partition` is requested to [readRecords](Partition.md#readRecords)
* `GroupMetadataManager` is requested to `doLoadGroupsAndOffsets`
* `TransactionStateManager` is requested to `loadTransactionMetadata`
* `Log` is requested to [convertToOffsetMetadataOrThrow](#convertToOffsetMetadataOrThrow)
* `KafkaMetadataLog` is requested to `read`

## Logging

Enable `ALL` logging level for `kafka.log.Log` logger to see what happens inside.

Add the following line to `log4j.properties`:

```text
log4j.logger.kafka.log.Log=ALL
```

Refer to [Logging](logging.md).
