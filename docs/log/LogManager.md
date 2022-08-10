# LogManager

![LogManager and KafkaServer](../images/LogManager.png)

## Creating Instance

`LogManager` takes the following to be created:

* <span id="logDirs"> [Log directories](../KafkaConfig.md#logDirs)
* <span id="initialOfflineDirs"> Initial offline directories
* <span id="configRepository"> `ConfigRepository`
* <span id="initialDefaultConfig"> `LogConfig`
* <span id="cleanerConfig"> `CleanerConfig`
* <span id="recoveryThreadsPerDataDir"> [num.recovery.threads.per.data.dir](../KafkaConfig.md#numRecoveryThreadsPerDataDir)
* <span id="flushCheckMs"> [log.flush.scheduler.interval.ms](../KafkaConfig.md#logFlushSchedulerIntervalMs)
* <span id="flushRecoveryOffsetCheckpointMs"> [log.flush.offset.checkpoint.interval.ms](../KafkaConfig.md#logFlushOffsetCheckpointIntervalMs)
* <span id="flushStartOffsetCheckpointMs"> [log.flush.start.offset.checkpoint.interval.ms](../KafkaConfig.md#logFlushStartOffsetCheckpointIntervalMs)
* <span id="retentionCheckMs"> [log.retention.check.interval.ms](../KafkaConfig.md#logCleanupIntervalMs)
* <span id="maxTransactionTimeoutMs"> [transaction.max.timeout.ms](../KafkaConfig.md#transactionMaxTimeoutMs)
* <span id="maxPidExpirationMs"> [transactional.id.expiration.ms](../KafkaConfig.md#transactionalIdExpirationMs)
* <span id="interBrokerProtocolVersion"> [inter.broker.protocol.version](../KafkaConfig.md#interBrokerProtocolVersion)
* <span id="scheduler"> `Scheduler`
* <span id="brokerTopicStats"> `BrokerTopicStats`
* <span id="logDirFailureChannel"> `LogDirFailureChannel`
* <span id="time"> `Time`
* <span id="keepPartitionMetadataFile"> `keepPartitionMetadataFile`

`LogManager` is created using [apply](#apply) factory method.

### <span id="apply"> Creating LogManager

```scala
apply(
  config: KafkaConfig,
  initialOfflineDirs: Seq[String],
  configRepository: ConfigRepository,
  kafkaScheduler: KafkaScheduler,
  time: Time,
  brokerTopicStats: BrokerTopicStats,
  logDirFailureChannel: LogDirFailureChannel,
  keepPartitionMetadataFile: Boolean): LogManager
```

`apply` [extracts log-related configuration properties](LogConfig.md#extractLogConfigMap) (from the given [KafkaConfig](../KafkaConfig.md)) and creates a [LogConfig](LogConfig.md).

`apply` creates a [LogCleaner](LogCleaner.md#cleanerConfig).

In the end, `apply` creates a [LogManager](#creating-instance) based on some [configuration properties](../KafkaConfig.md).

---

`apply` is used when:

* `BrokerServer` is requested to [start up](../raft/BrokerServer.md#logManager)
* `KafkaServer` is requested to [start up](../broker/KafkaServer.md#logManager)

## <span id="KafkaMetricsGroup"> KafkaMetricsGroup

`LogManager` is a [KafkaMetricsGroup](../metrics/KafkaMetricsGroup.md).

## Logging

Enable `ALL` logging level for `kafka.log.LogManager` logger to see what happens inside.

Add the following line to `conf/log4j.properties`:

```text
log4j.logger.kafka.log.LogManager=ALL
```

Refer to [Logging](../logging.md).
