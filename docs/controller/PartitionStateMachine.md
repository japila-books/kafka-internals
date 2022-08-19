# PartitionStateMachine

`PartitionStateMachine` is an [abstraction](#contract) of [partition state machines](#implementations) that can [handleStateChanges](#handleStateChanges).

## Contract

### <span id="handleStateChanges"> handleStateChanges

```scala
handleStateChanges(
  partitions: Seq[TopicPartition],
  targetState: PartitionState
): Map[TopicPartition, Either[Throwable, LeaderAndIsr]] // (1)!
handleStateChanges(
  partitions: Seq[TopicPartition],
  targetState: PartitionState,
  leaderElectionStrategy: Option[PartitionLeaderElectionStrategy]
): Map[TopicPartition, Either[Throwable, LeaderAndIsr]]
```

1. Uses an undefined `leaderElectionStrategy` (`None`)

Handles state changes of partitions (_partition state changes_)

Used when:

* `KafkaController` is requested to [onNewPartitionCreation](KafkaController.md#onNewPartitionCreation), [onReplicasBecomeOffline](KafkaController.md#onReplicasBecomeOffline), [onReplicaElection](KafkaController.md#onReplicaElection), [moveReassignedPartitionLeaderIfRequired](KafkaController.md#moveReassignedPartitionLeaderIfRequired), [doControlledShutdown](KafkaController.md#doControlledShutdown)
* `PartitionStateMachine` is requested to [triggerOnlineStateChangeForPartitions](#triggerOnlineStateChangeForPartitions)
* `TopicDeletionManager` is requested to [onTopicDeletion](TopicDeletionManager.md#onTopicDeletion)

## Implementations

* [ZkPartitionStateMachine](ZkPartitionStateMachine.md)

## Creating Instance

`PartitionStateMachine` takes the following to be created:

* <span id="controllerContext"> [ControllerContext](ControllerContext.md)

!!! note "Abstract Class"
    `PartitionStateMachine` is an abstract class and cannot be created directly. It is created indirectly for the [concrete PartitionStateMachines](#implementations).

## <span id="startup"> Starting Up (on Active Controller)

```scala
startup(): Unit
```

---

`startup` prints out the following INFO message to the logs:

```text
Initializing partition state
```

`startup` [initializePartitionState](#initializePartitionState).

`startup` prints out the following INFO message to the logs:

```text
Triggering online partition state changes
```

`startup` [triggerOnlinePartitionStateChange](#triggerOnlinePartitionStateChange).

In the end, `startup` prints out the following DEBUG message to the logs:

```text
Started partition state machine with initial state -> [partitionStates]
```

---

`startup` is used when:

* `KafkaController` is requested to [onControllerFailover](KafkaController.md#onControllerFailover) (when a broker is successfully elected as the controller)

### <span id="initializePartitionState"> initializePartitionState

```scala
initializePartitionState(): Unit
```

---

`initializePartitionState`...FIXME

## <span id="triggerOnlinePartitionStateChange"> triggerOnlinePartitionStateChange

```scala
triggerOnlinePartitionStateChange(): Map[TopicPartition, Either[Throwable, LeaderAndIsr]]
triggerOnlinePartitionStateChange(
  topic: String): Unit
```

---

`triggerOnlinePartitionStateChange`...FIXME

`triggerOnlinePartitionStateChange` is used when:

* `KafkaController` is requested to [onBrokerStartup](KafkaController.md#onBrokerStartup), [onReplicasBecomeOffline](KafkaController.md#onReplicasBecomeOffline), [processUncleanLeaderElectionEnable](KafkaController.md#processUncleanLeaderElectionEnable), [processTopicUncleanLeaderElectionEnable](KafkaController.md#processTopicUncleanLeaderElectionEnable)
* `PartitionStateMachine` is requested to [start up](#startup)

## Review Me

=== [[PartitionLeaderElectionStrategy]] PartitionLeaderElectionStrategy

.PartitionLeaderElectionStrategies
[cols="30m,70",options="header",width="100%"]
|===
| Name
| Description

| ControlledShutdownPartitionLeaderElectionStrategy
a| [[ControlledShutdownPartitionLeaderElectionStrategy]]

| OfflinePartitionLeaderElectionStrategy
a| [[OfflinePartitionLeaderElectionStrategy]] Accepts `allowUnclean` flag

Handled by `ZkPartitionStateMachine` when requested to link:kafka-controller-ZkPartitionStateMachine.adoc#doElectLeaderForPartitions[doElectLeaderForPartitions]

Used when:

* `KafkaController` is requested to link:kafka-controller-KafkaController.adoc#onNewPartitionCreation[onNewPartitionCreation] (with the `allowUnclean` flag off), link:kafka-controller-KafkaController.adoc#onReplicaElection[onReplicaElection] (with the `allowUnclean` flag on for the admin client)

* `PartitionStateMachine` is requested to <<triggerOnlineStateChangeForPartitions, triggerOnlineStateChangeForPartitions>> (with the `allowUnclean` flag off)

| PreferredReplicaPartitionLeaderElectionStrategy
a| [[PreferredReplicaPartitionLeaderElectionStrategy]] `KafkaController` is requested for <<kafka-controller-KafkaController.adoc#onPreferredReplicaElection, preferred replica leader election>> that in turn triggers `ZkPartitionStateMachine` to <<kafka-controller-ZkPartitionStateMachine.adoc#leaderForPreferredReplica, leaderForPreferredReplica>>

| ReassignPartitionLeaderElectionStrategy
a| [[ReassignPartitionLeaderElectionStrategy]]

|===

=== [[triggerOnlinePartitionStateChange]] `triggerOnlinePartitionStateChange` Method

[source, scala]
----
triggerOnlinePartitionStateChange(): Unit
triggerOnlinePartitionStateChange(
  topic: String): Unit // <1>
----
<1> Uses the partitions of the given topic only

`triggerOnlinePartitionStateChange` requests the <<controllerContext, ControllerContext>> for the link:kafka-controller-ControllerContext.adoc#partitionsInStates[partitions in the states]: `OfflinePartition` and `NewPartition`.

With the topic specified, `triggerOnlinePartitionStateChange` requests the <<controllerContext, ControllerContext>> for the link:kafka-controller-ControllerContext.adoc#partitionsInStates[partitions of the topic in the states]: `OfflinePartition` and `NewPartition`.

In the end, `triggerOnlinePartitionStateChange` <<triggerOnlineStateChangeForPartitions, triggers online state change for the partitions>>.

[NOTE]
====
`triggerOnlinePartitionStateChange` is used when:

* `KafkaController` is requested to link:kafka-controller-KafkaController.adoc#onBrokerStartup[onBrokerStartup], link:kafka-controller-KafkaController.adoc#onReplicasBecomeOffline[onReplicasBecomeOffline], link:kafka-controller-KafkaController.adoc#processUncleanLeaderElectionEnable[processUncleanLeaderElectionEnable], link:kafka-controller-KafkaController.adoc#processTopicUncleanLeaderElectionEnable[processTopicUncleanLeaderElectionEnable]

* `PartitionStateMachine` is requested to <<startup, start up>>
====

=== [[triggerOnlineStateChangeForPartitions]] `triggerOnlineStateChangeForPartitions` Internal Method

[source, scala]
----
triggerOnlineStateChangeForPartitions(
  partitions: collection.Set[TopicPartition]): Unit
----

`triggerOnlineStateChangeForPartitions` filters out the partitions of the link:kafka-controller-ControllerContext.adoc#isTopicQueuedUpForDeletion[topics to be deleted] and tries to <<handleStateChanges, move the partitions>> to `OnlinePartition` state with <<OfflinePartitionLeaderElectionStrategy, OfflinePartitionLeaderElectionStrategy>> (with `allowUnclean` flag off).

NOTE: `triggerOnlineStateChangeForPartitions` is used when `PartitionStateMachine` is requested to <<triggerOnlinePartitionStateChange, triggerOnlinePartitionStateChange>>.

=== [[shutdown]] Shutting Down -- `shutdown` Method

[source, scala]
----
shutdown(): Unit
----

`shutdown` simply prints out the following INFO message to the logs:

```
Stopped partition state machine
```

NOTE: `shutdown` is used exclusively when is requested to <<kafka-controller-KafkaController.adoc#onControllerResignation, resign as the active controller>>

=== [[initializePartitionState]] `initializePartitionState` Internal Method

[source, scala]
----
initializePartitionState(): Unit
----

`initializePartitionState` requests the <<controllerContext, ControllerContext>> for <<kafka-controller-ControllerContext.adoc#allPartitions, all partitions>> (across all the brokers in the Kafka cluster).

For every `TopicPartition`, `initializePartitionState` requests the <<controllerContext, ControllerContext>> for the `LeaderIsrAndControllerEpoch` metadata (using the <<kafka-controller-ControllerContext.adoc#partitionLeadershipInfo, partitionLeadershipInfo>> internal registry).

`initializePartitionState` <<changeStateTo, changeStateTo>> of a `TopicPartition` as follows:

* `OnlinePartition` when the <<controllerContext, ControllerContext>> says that the <<kafka-controller-ControllerContext.adoc#isReplicaOnline, replica is online>> (for the leader ISR and the `TopicPartition`)

* `OfflinePartition` when the <<controllerContext, ControllerContext>> says that the <<kafka-controller-ControllerContext.adoc#isReplicaOnline, replica is not online>> (for the leader ISR and the `TopicPartition`)

* `NewPartition` when the <<controllerContext, ControllerContext>> has no metadata about the `TopicPartition`

NOTE: `initializePartitionState` is used exclusively when `PartitionStateMachine` is requested to <<startup, start up on the active controller>>.
