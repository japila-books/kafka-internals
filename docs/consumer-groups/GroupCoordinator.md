# GroupCoordinator

![GroupCoordinator's Startup](../images/GroupCoordinator-startup.png)

## Creating Instance

`GroupCoordinator` takes the following to be created:

* <span id="brokerId"> [broker.id](../KafkaConfig.md#brokerId)
* <span id="groupConfig"> [GroupConfig](GroupConfig.md)
* <span id="offsetConfig"> [OffsetConfig](OffsetConfig.md)
* <span id="groupManager"> [GroupMetadataManager](GroupMetadataManager.md)
* <span id="heartbeatPurgatory"> `DelayedOperationPurgatory[DelayedHeartbeat]`
* <span id="rebalancePurgatory"> `DelayedOperationPurgatory[DelayedRebalance]`
* <span id="time"> `Time`
* <span id="metrics"> [Metrics](../metrics/Metrics.md)

`GroupCoordinator` is created using [apply](#apply) factory.

## <span id="apply"> Creating GroupCoordinator Instance

```scala
apply(
  config: KafkaConfig,
  replicaManager: ReplicaManager,
  time: Time,
  metrics: Metrics): GroupCoordinator // (1)!
apply(
  config: KafkaConfig,
  replicaManager: ReplicaManager,
  heartbeatPurgatory: DelayedOperationPurgatory[DelayedHeartbeat],
  rebalancePurgatory: DelayedOperationPurgatory[DelayedRebalance],
  time: Time,
  metrics: Metrics): GroupCoordinator
```

1. Creates `DelayedOperationPurgatory`s and calls the other `apply`

!!! note
    All `GroupCoordinator` really needs for work is [ReplicaManager](../ReplicaManager.md).

`apply` [creates a OffsetConfig](#offsetConfig) (based on the given [KafkaConfig](../KafkaConfig.md)).

`apply` creates a [GroupConfig](GroupConfig.md) based on the following configuration properties (in the [KafkaConfig](../KafkaConfig.md)):

* [groupMinSessionTimeoutMs](../KafkaConfig.md#groupMinSessionTimeoutMs)
* [groupMaxSessionTimeoutMs](../KafkaConfig.md#groupMaxSessionTimeoutMs)
* [groupMaxSize](../KafkaConfig.md#groupMaxSize)
* [groupInitialRebalanceDelay](../KafkaConfig.md#groupInitialRebalanceDelay)

`apply` creates a [GroupMetadataManager](GroupMetadataManager.md) based on the following configuration properties (in the [KafkaConfig](../KafkaConfig.md)):

* [brokerId](../KafkaConfig.md#brokerId)
* [interBrokerProtocolVersion](../KafkaConfig.md#interBrokerProtocolVersion)

In the end, `apply` creates a [GroupCoordinator](GroupCoordinator.md).

---

`apply` is used when:

* `BrokerServer` is requested to [start up](../raft/BrokerServer.md#groupCoordinator)
* `KafkaServer` is requested to [start up](../broker/KafkaServer.md#groupCoordinator)

## Logging

Enable `ALL` logging level for `kafka.coordinator.group.GroupCoordinator` logger to see what happens inside.

Add the following line to `conf/log4j.properties`:

```text
log4j.logger.kafka.coordinator.group.GroupCoordinator=ALL
```

Refer to [Logging](../logging.md).
