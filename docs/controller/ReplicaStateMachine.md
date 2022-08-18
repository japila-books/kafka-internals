# ReplicaStateMachine

## Review Me

`ReplicaStateMachine` is the <<contract, base>> of <<extensions, Replica State Machines>> that can <<handleStateChanges, handleStateChanges>>, be <<startup, started up>> and <<shutdown, shut down>> in the end.

[[contract]]
.ReplicaStateMachine Contract (Abstract Methods Only)
[cols="30m,70",options="header",width="100%"]
|===
| Method
| Description

| handleStateChanges
a| [[handleStateChanges]]

[source, scala]
----
handleStateChanges(
  replicas: Seq[PartitionAndReplica],
  targetState: ReplicaState): Unit
----

Handles state changes of replicas (_replica state changes_)

Used when:

* `KafkaController` is requested to <<kafka-controller-KafkaController.adoc#onBrokerLogDirFailure, onBrokerLogDirFailure>>, <<kafka-controller-KafkaController.adoc#onBrokerStartup, onBrokerStartup>>, <<kafka-controller-KafkaController.adoc#onReplicasBecomeOffline, onReplicasBecomeOffline>>, <<kafka-controller-KafkaController.adoc#onNewPartitionCreation, onNewPartitionCreation>>, <<kafka-controller-KafkaController.adoc#onPartitionReassignment, onPartitionReassignment>>, <<kafka-controller-KafkaController.adoc#stopOldReplicasOfReassignedPartition, stopOldReplicasOfReassignedPartition>>, <<kafka-controller-KafkaController.adoc#startNewReplicasForReassignedPartition, startNewReplicasForReassignedPartition>>, and <<kafka-controller-KafkaController.adoc#doControlledShutdown, doControlledShutdown>>

* `ReplicaStateMachine` is requested to <<startup, start up>> (when `KafkaController` is requested to <<kafka-controller-KafkaController.adoc#onControllerFailover, onControllerFailover>> on a broker elected as the controller)

* `TopicDeletionManager` is requested to <<kafka-controller-TopicDeletionManager.adoc#failReplicaDeletion, failReplicaDeletion>>, <<kafka-controller-TopicDeletionManager.adoc#completeReplicaDeletion, completeReplicaDeletion>>, <<kafka-controller-TopicDeletionManager.adoc#retryDeletionForIneligibleReplicas, retryDeletionForIneligibleReplicas>>, <<kafka-controller-TopicDeletionManager.adoc#completeDeleteTopic, completeDeleteTopic>>, and <<kafka-controller-TopicDeletionManager.adoc#startReplicaDeletion, startReplicaDeletion>>

|===

[[implementations]]
NOTE: <<kafka-controller-ZkReplicaStateMachine.adoc#, ZkReplicaStateMachine>> is the default and only known implementation of the <<contract, ReplicaStateMachine Contract>> in Apache Kafka.

[[creating-instance]][[controllerContext]]
`ReplicaStateMachine` takes a single <<kafka-controller-ControllerContext.adoc#, ControllerContext>> to be created.

NOTE: `ReplicaStateMachine` is a Scala abstract class and cannot be <<creating-instance, created>> directly. It is created indirectly for the <<implementations, concrete ReplicaStateMachines>>.

=== [[shutdown]] Shutting Down -- `shutdown` Method

[source, scala]
----
shutdown(): Unit
----

`shutdown` simply prints out the following INFO message to the logs:

```
Stopped replica state machine
```

NOTE: `shutdown` is used exclusively when `KafkaController` is requested to <<kafka-controller-KafkaController.adoc#onControllerResignation, resign as the active controller>>.

=== [[startup]] Starting Up (On Active Controller) -- `startup` Method

[source, scala]
----
startup(): Unit
----

`startup` prints out the following INFO message to the logs:

```
Initializing replica state
```

`startup` <<initializeReplicaState, initializeReplicaState>>.

`startup` prints out the following INFO message to the logs:

```
Triggering online replica state changes
```

`startup` requests the <<kafka-controller-ControllerContext.adoc#, ControllerContext>> for <<kafka-controller-ControllerContext.adoc#onlineAndOfflineReplicas, online and offline replicas>>.

`startup` <<handleStateChanges, handleStateChanges>> (with the online replicas and `OnlineReplica` target state).

`startup` prints out the following INFO message to the logs:

```
Triggering offline replica state changes
```

`startup` <<handleStateChanges, handleStateChanges>> (with the offline replicas and `OfflineReplica` target state).

In the end, `startup` prints out the following DEBUG message to the logs:

```
Started replica state machine with initial state -> [replicaState]
```

NOTE: `startup` is used exclusively when `KafkaController` is requested to <<kafka-controller-KafkaController.adoc#onControllerFailover, onControllerFailover>> (when a broker is successfully elected as the controller).

=== [[initializeReplicaState]] Initializing Partition Replica States using ControllerContext -- `initializeReplicaState` Internal Method

[source, scala]
----
initializeReplicaState(): Unit
----

`initializeReplicaState` requests the <<controllerContext, ControllerContext>> for <<kafka-controller-ControllerContext.adoc#allPartitions, all partitions>>.

For every partition, `initializeReplicaState` requests the <<controllerContext, ControllerContext>> for the <<kafka-controller-ControllerContext.adoc#partitionReplicaAssignment, partition replica assignment>> (the broker IDs of partition replicas).

For every partition replica, `initializeReplicaState` requests the <<controllerContext, ControllerContext>> to <<putReplicaState, putReplicaState>> to `OnlineReplica` or `ReplicaDeletionIneligible` per <<kafka-controller-ControllerContext.adoc#isReplicaOnline, isReplicaOnline>>.

NOTE: `initializeReplicaState` is used exclusively when `ReplicaStateMachine` is requested to <<startup, start up>>.
