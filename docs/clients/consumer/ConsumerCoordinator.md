# ConsumerCoordinator

`ConsumerCoordinator` is a [consumer group coordination manager](AbstractCoordinator.md).

## Creating Instance

`ConsumerCoordinator` takes the following to be created:

* <span id="rebalanceConfig"> `GroupRebalanceConfig`
* <span id="logContext"> `LogContext`
* <span id="client"> [ConsumerNetworkClient](ConsumerNetworkClient.md)
* <span id="assignors"> [ConsumerPartitionAssignor](ConsumerPartitionAssignor.md)s
* <span id="metadata"> [ConsumerMetadata](ConsumerMetadata.md)
* <span id="subscriptions"> [SubscriptionState](SubscriptionState.md)
* <span id="metrics"> `Metrics`
* <span id="metricGrpPrefix"> Metrics Group Prefix
* <span id="time"> `Time`
* [autoCommitEnabled](#autoCommitEnabled)
* <span id="autoCommitIntervalMs"> [auto.commit.interval.ms](ConsumerConfig.md#AUTO_COMMIT_INTERVAL_MS_CONFIG)
* <span id="interceptors"> `ConsumerInterceptors`
* <span id="throwOnFetchStableOffsetsUnsupported"> [internal.throw.on.fetch.stable.offset.unsupported](ConsumerConfig.md#THROW_ON_FETCH_STABLE_OFFSET_UNSUPPORTED)

While being created, `ConsumerCoordinator` requests the [ConsumerMetadata](#metadata) for an [update of the current cluster metadata](../Metadata.md#requestUpdate).

`ConsumerCoordinator` is created when:

* `KafkaConsumer` is [created](KafkaConsumer.md#coordinator) (and [group.id](KafkaConsumer.md#groupId) configuration property is specified)

## <span id="autoCommitEnabled"> autoCommitEnabled

`ConsumerCoordinator` is given `autoCommitEnabled` flag when [created](#creating-instance) with the value based on [group.id and enable.auto.commit configuration properties](ConsumerConfig.md#maybeOverrideEnableAutoCommit).

## <span id="metadata"> metadata

```java
JoinGroupRequestData.JoinGroupRequestProtocolCollection metadata()
```

`metadata`...FIXME

`metadata` is part of the [AbstractCoordinator](AbstractCoordinator.md#metadata) abstraction.
