# LogConfig

## <span id="CleanupPolicyProp"><span id="cleanup.policy"> cleanup.policy

[TopicConfig](../TopicConfig.md#CLEANUP_POLICY_CONFIG)

### <span id="compact"> compact

`compact` is `true` when [cleanup.policy](#CleanupPolicyProp) contains `compact` policy.

---

`compact` is used when:

* `LogCleanerManager` is requested to [grabFilthiestCompactedLog](LogCleanerManager.md#grabFilthiestCompactedLog), [pauseCleaningForNonCompactedPartitions](LogCleanerManager.md#pauseCleaningForNonCompactedPartitions), [deletableLogs](LogCleanerManager.md#deletableLogs), [maybeTruncateCheckpoint](LogCleanerManager.md#maybeTruncateCheckpoint), [isCompactAndDelete](LogCleanerManager.md#isCompactAndDelete)
* `LogConfig` is requested for [maxSegmentMs](#maxSegmentMs)
* `LogManager` is requested to [updateTopicConfig](LogManager.md#updateTopicConfig) and [cleanupLogs](LogManager.md#cleanupLogs)
* `UnifiedLog` is requested to [append](UnifiedLog.md#append)

## <span id="LeaderReplicationThrottledReplicasProp"><span id="leader.replication.throttled.replicas"><span id="LeaderReplicationThrottledReplicas"> leader.replication.throttled.replicas

A list of replicas for which log replication should be throttled on the leader side.

The list should describe a set of replicas in the form `[PartitionId]:[BrokerId],[PartitionId]:[BrokerId]:...` or alternatively the wildcard `*` can be used to throttle all replicas for this topic.

Default: (empty)

Use `LogConfig.LeaderReplicationThrottledReplicas` to access the current value

Used when:

* `ReassignPartitionsCommand` is [created](../tools/kafka-reassign-partitions/ReassignPartitionsCommand.md#topicLevelLeaderThrottle)
* `TopicConfigHandler` is requested to [processConfigChanges](../dynamic-broker-configuration/TopicConfigHandler.md#processConfigChanges)

## <span id="FollowerReplicationThrottledReplicasProp"><span id="follower.replication.throttled.replicas"><span id="FollowerReplicationThrottledReplicas"> follower.replication.throttled.replicas

A list of replicas for which log replication should be throttled on the follower side.

The list should describe a set of replicas in the form `[PartitionId]:[BrokerId],[PartitionId]:[BrokerId]:...` or alternatively the wildcard `*` can be used to throttle all replicas for this topic.

Default: (empty)

Use `LogConfig.FollowerReplicationThrottledReplicas` to access the current value

Used when:

* `ReassignPartitionsCommand` is [created](../tools/kafka-reassign-partitions/ReassignPartitionsCommand.md#topicLevelFollowerThrottle)
* `TopicConfigHandler` is requested to [processConfigChanges](../dynamic-broker-configuration/TopicConfigHandler.md#processConfigChanges)

## <span id="UncleanLeaderElectionEnableProp"><span id="unclean.leader.election.enable"><span id="uncleanLeaderElectionEnable"> unclean.leader.election.enable

[TopicConfig](../TopicConfig.md#UNCLEAN_LEADER_ELECTION_ENABLE_CONFIG)

Default: `false` (disabled)

Use `LogConfig.uncleanLeaderElectionEnable` to access the current value.

[KafkaConfig](../KafkaConfig.md#UncleanLeaderElectionEnableProp)

`TransactionStateManager` disables the property explicitly (`false`) for [__transaction_state](../transactions/TransactionStateManager.md#transactionTopicConfigs) topic.

Used in [extractLogConfigMap](#extractLogConfigMap)

## Utilities

### <span id="extractLogConfigMap"> extractLogConfigMap

```scala
extractLogConfigMap(
  kafkaConfig: KafkaConfig): Map[String, Object]
```

`extractLogConfigMap`...FIXME

`extractLogConfigMap` is used when:

* `LogManager` is [created](LogManager.md#apply)
* `ConfigHelper` is requested to `describeConfigs`
* `ZkAdminManager` is requested to [maybePopulateMetadataAndConfigs](../ZkAdminManager.md#maybePopulateMetadataAndConfigs)
