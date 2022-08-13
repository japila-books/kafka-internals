# Log Cleanup

Kafka uses [cleanup.policy](#cleanup.policy) configuration property to apply **cleanup strategies** (policy) to logs:

* [Log Compaction](#log-compaction)
* [Log Retention](#log-retention)

## <span id="log.cleanup.policy"><span id="cleanup.policy"> Configuration Properties

The cluster-wide [log.cleanup.policy](../KafkaConfig.md#log.cleanup.policy) and the per-topic [cleanup.policy](../TopicConfig.md#cleanup.policy) configuration properties are comma-separated lists of cleanup strategies:

* <span id="compact"> `compact` - enables [log compaction](#log-compaction)
* <span id="delete"> `delete` - enables [log retention](#log-retention)

Unless defined, [cleanup.policy](../TopicConfig.md#cleanup.policy) is exactly [log.cleanup.policy](../KafkaConfig.md#log.cleanup.policy).

## Log Compaction

**Log Compaction** is a cleanup strategy in which...FIXME

Kafka brokers use [LogCleaner](../log/LogCleaner.md) for [compact](#compact) retention strategy.

Log compaction can be [reconfigured dynamically at runtime](../log/CleanerConfig.md).

## Log Retention

**Log Retention** (_Garbage Collection_) is a cleanup strategy to discard (_delete_) old log segments when their [retention time](#time-based-retention) or [size limit](#size-based-retention) has been reached.

By default there is only a time limit and no size limit.

Kafka brokers schedule `kafka-log-retention` periodic task for [delete](#delete) retention strategy.

Kafka uses [log.retention.check.interval.ms](../KafkaConfig.md#log.retention.check.interval.ms) configuration property as the interval between regular log checks.

### Time-Based Retention

**Retention Time** is controlled by the cluster-wide [log.retention.ms](../KafkaConfig.md#log.retention.ms), [log.retention.minutes](../KafkaConfig.md#log.retention.minutes) or [log.retention.hours](../KafkaConfig.md#log.retention.hours) configuration properties (from the highest to the lowest priority) or their per-topic [retention.ms](../TopicConfig.md#retention.ms) configuration property.

### Size-Based Retention

**Retention Size** is controlled by the cluster-wide [log.retention.bytes](../KafkaConfig.md#log.retention.bytes) or per-topic [retention.bytes](../TopicConfig.md#retention.bytes) configuration property.

## Logging

Enable `ALL` logging level for [kafka.log.Log](../Log.md#logging) logger to see messages related to log retention.
