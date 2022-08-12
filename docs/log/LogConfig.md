# LogConfig

## <span id="CleanupPolicyProp"><span id="cleanup.policy"> cleanup.policy

[cleanup.policy](../TopicConfig.md#CLEANUP_POLICY_CONFIG)

### <span id="compact"> compact

`compact` is `true` when [cleanup.policy](#CleanupPolicyProp) contains `compact` policy.

---

`compact` is used when:

* `LogCleanerManager` is requested to [grabFilthiestCompactedLog](LogCleanerManager.md#grabFilthiestCompactedLog), [pauseCleaningForNonCompactedPartitions](LogCleanerManager.md#pauseCleaningForNonCompactedPartitions), [deletableLogs](LogCleanerManager.md#deletableLogs), [maybeTruncateCheckpoint](LogCleanerManager.md#maybeTruncateCheckpoint), [isCompactAndDelete](LogCleanerManager.md#isCompactAndDelete)
* `LogConfig` is requested for [maxSegmentMs](#maxSegmentMs)
* `LogManager` is requested to [updateTopicConfig](LogManager.md#updateTopicConfig) and [cleanupLogs](LogManager.md#cleanupLogs)
* `UnifiedLog` is requested to [append](UnifiedLog.md#append)

## <span id="extractLogConfigMap"> extractLogConfigMap

```scala
extractLogConfigMap(
  kafkaConfig: KafkaConfig): ju.Map[String, Object]
```

`extractLogConfigMap`...FIXME

`extractLogConfigMap` is used when:

* FIXME
