# UnifiedLog

## Creating Instance

`UnifiedLog` takes the following to be created:

* <span id="logStartOffset"> `logStartOffset`
* <span id="localLog"> [LocalLog](LocalLog.md)
* <span id="brokerTopicStats"> `BrokerTopicStats`
* <span id="producerIdExpirationCheckIntervalMs"> `producerIdExpirationCheckIntervalMs`
* <span id="leaderEpochCache"> `LeaderEpochFileCache`
* <span id="producerStateManager"> `ProducerStateManager`
* <span id="_topicId"> Topic ID
* <span id="keepPartitionMetadataFile"> `keepPartitionMetadataFile`
* <span id="remoteStorageSystemEnable"> `remoteStorageSystemEnable` (default: `false`)
* <span id="logOffsetsListener"> `LogOffsetsListener`

`UnifiedLog` is created using [apply](#apply) utility.

## Creating UnifiedLog { #apply }

```scala
apply(
  dir: File,
  config: LogConfig,
  logStartOffset: Long,
  recoveryPoint: Long,
  scheduler: Scheduler,
  brokerTopicStats: BrokerTopicStats,
  time: Time,
  maxTransactionTimeoutMs: Int,
  producerStateManagerConfig: ProducerStateManagerConfig,
  producerIdExpirationCheckIntervalMs: Int,
  logDirFailureChannel: LogDirFailureChannel,
  lastShutdownClean: Boolean = true,
  topicId: Option[Uuid],
  keepPartitionMetadataFile: Boolean,
  numRemainingSegments: ConcurrentMap[String, Int] = new ConcurrentHashMap[String, Int],
  remoteStorageSystemEnable: Boolean = false,
  logOffsetsListener: LogOffsetsListener = LogOffsetsListener.NO_OP_OFFSETS_LISTENER): UnifiedLog
```

`apply`...FIXME

---

`apply` is used when:

* `LogManager` is requested to [startup](LogManager.md#startup) (and [loadLog](LogManager.md#loadLog)), [getOrCreateLog](LogManager.md#getOrCreateLog)
* `KafkaMetadataLog` is requested to [create a KafkaMetadataLog](../kraft/KafkaMetadataLog.md#apply)
