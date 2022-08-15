# ControllerContext

`ControllerContext` is the context of an active <<kafka-controller-KafkaController.md#, KafkaController>> (and is <<creating-instance, created>> right when `KafkaController` is <<kafka-controller-KafkaController.md#controllerContext, created>>).

[[creating-instance]]
`ControllerContext` takes no input arguments to be created.

=== [[allPartitions]] `allPartitions` Method

[source, scala]
----
allPartitions: Set[TopicPartition]
----

`allPartitions` converts the <<partitionReplicaAssignmentUnderlying, partitionReplicaAssignmentUnderlying>> into `TopicPartitions`, i.e. `allPartitions` takes the partitions for the topics and simply creates new `TopicPartitions`.

[NOTE]
====
`allPartitions` is used when:

* `KafkaController` is requested to <<kafka-controller-KafkaController.md#updateLeaderAndIsrCache, updateLeaderAndIsrCache>>, <<kafka-controller-KafkaController.md#checkAndTriggerAutoLeaderRebalance, checkAndTriggerAutoLeaderRebalance>>, and <<kafka-controller-KafkaController.md#updateMetrics, updateMetrics>>

* `PartitionStateMachine` is requested to <<kafka-controller-PartitionStateMachine.md#initializePartitionState, initializePartitionState>>

* `ReplicaStateMachine` is requested to <<kafka-controller-ReplicaStateMachine.md#initializeReplicaState, initializeReplicaState>>
====

=== [[updatePartitionReplicaAssignment]] `updatePartitionReplicaAssignment` Method

[source, scala]
----
updatePartitionReplicaAssignment(topicPartition: TopicPartition, newReplicas: Seq[Int]): Unit
----

`updatePartitionReplicaAssignment` simply updates the <<partitionReplicaAssignmentUnderlying, partitionReplicaAssignmentUnderlying>> registry with `newReplicas` for the topic and the partition (of a given `TopicPartition`).

[NOTE]
====
`updatePartitionReplicaAssignment` is used when:

* `KafkaController` is requested to <<kafka-controller-KafkaController.md#initializeControllerContext, initializeControllerContext>>, <<kafka-controller-KafkaController.md#moveReassignedPartitionLeaderIfRequired, moveReassignedPartitionLeaderIfRequired>>, <<kafka-controller-KafkaController.md#updateAssignedReplicasForPartition, updateAssignedReplicasForPartition>>, and at <<kafka-controller-ControllerEvent.md#TopicChange, TopicChange>> and <<kafka-controller-ControllerEvent.md#PartitionModifications, PartitionModifications>> controller events

* `ReplicaStateMachine` is requested to <<kafka-controller-ReplicaStateMachine.md#doHandleStateChanges, doHandleStateChanges>>
====

=== [[partitionReplicaAssignment]] `partitionReplicaAssignment` Method

[source, scala]
----
partitionReplicaAssignment(
  topicPartition: TopicPartition): Seq[Int]
----

`partitionReplicaAssignment` finds the brokers with the replicas of the given partition (aka _partition replica assignment_).

Internally, `partitionReplicaAssignment` finds broker IDs of the replicas of the given partition (`TopicPartition`) in the <<partitionAssignments, partitionAssignments>> internal registry.

`partitionReplicaAssignment` returns an empty collection when no topic or partition are found.

NOTE: `partitionReplicaAssignment` is used when...FIXME

=== [[putReplicaStateIfNotExists]] `putReplicaStateIfNotExists` Method

[source, scala]
----
putReplicaStateIfNotExists(
  replica: PartitionAndReplica,
  state: ReplicaState): Unit
----

`putReplicaStateIfNotExists` simply adds the replica to the <<replicaStates, replicaStates>> internal registry unless available already.

NOTE: `putReplicaStateIfNotExists` is used exclusively when `ZkReplicaStateMachine` is requested to <<kafka-controller-ZkReplicaStateMachine.md#doHandleStateChanges, handle state changes of partition replicas>>.

=== [[checkValidReplicaStateChange]] `checkValidReplicaStateChange` Method

[source, scala]
----
checkValidReplicaStateChange(
  replicas: Seq[PartitionAndReplica],
  targetState: ReplicaState
): (Seq[PartitionAndReplica], Seq[PartitionAndReplica])
----

For every replica (in the given replicas), `checkValidReplicaStateChange` <<isValidReplicaStateTransition, isValidReplicaStateTransition>> with the target state (`ReplicaState`).

NOTE: `checkValidReplicaStateChange` is used exclusively when `ZkReplicaStateMachine` is requested to <<kafka-controller-ZkReplicaStateMachine.md#doHandleStateChanges, handle replica state changes>>.

=== [[checkValidPartitionStateChange]] `checkValidPartitionStateChange` Method

[source, scala]
----
checkValidPartitionStateChange(
  partitions: Seq[TopicPartition],
  targetState: PartitionState
): (Seq[TopicPartition], Seq[TopicPartition])
----

For every replica (in the given replicas), `checkValidPartitionStateChange` <<isValidPartitionStateTransition, isValidPartitionStateTransition>> with the target state (`PartitionState`).

NOTE: `checkValidPartitionStateChange` is used exclusively when `ZkReplicaStateMachine` is requested to <<kafka-controller-ZkPartitionStateMachine.md#doHandleStateChanges, handle partition state changes>>.

=== [[partitionsInStates]] Finding Partitions by Given States -- `partitionsInStates` Method

[source, scala]
----
partitionsInStates(
  states: Set[PartitionState]): Set[TopicPartition]
partitionsInStates(
  topic: String, states: Set[PartitionState]): Set[TopicPartition]
----

`partitionsInStates` uses the <<partitionStates, partitionStates>> internal registry to find all of the `TopicPartitions` (of the topic if defined) in the given `PartitionStates`.

NOTE: `partitionsInStates` is used when `PartitionStateMachine` is requested to link:kafka-controller-PartitionStateMachine.md#triggerOnlinePartitionStateChange[triggerOnlinePartitionStateChange].

## <span id="stats"><span id="rateAndTimeMetrics"><span id="ControllerStats"> ControllerStats

`ControllerStats` with `UncleanLeaderElectionsPerSec` meter metric and `KafkaTimers` for every [ControllerState](ControllerState.md) (except [Idle](ControllerState.md#Idle) state)

`stats` is used exclusively to create the <<kafka-controller-KafkaController.md#eventManager, ControllerEventManager>> (of <<kafka-controller-KafkaController.md#, KafkaController>>) that is then used to collect the times (metrics) of <<kafka-controller-ControllerEventThread.md#doWork, processing every ControllerEvent>> (except <<kafka-controller-ControllerEvent.md#ShutdownEventThread, ShutdownEventThread>>)

* Every `ControllerState` has the <<kafka-controller-ControllerState.md#rateAndTimeMetricName, RateAndTimeMs>> metric defined (except <<kafka-controller-ControllerState.md#Idle, Idle>> state)

The timer metric name pattern is **kafka.controller:type=ControllerStats,name=**.
