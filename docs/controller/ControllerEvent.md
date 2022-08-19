# ControllerEvent

`ControllerEvent` is an [abstraction](#contract) of [events](#implementations) in the lifecycle of [KafkaController](KafkaController.md) (state machine) that trigger [state](#state) change.

`ControllerEvent` events are managed by [ControllerEventManager](ControllerEventManager.md).

## Contract

### <span id="preempt"> preempt

```scala
preempt(): Unit
```

Used when:

* FIXME

### <span id="state"> ControllerState

```scala
state: ControllerState
```

[ControllerState](ControllerState.md) of the [ControllerEventManager](ControllerEventManager.md) (while [processing controller events](ControllerEventThread.md#doWork))

Used when:

* FIXME

## Implementations

* AllocateProducerIds
* AlterPartitionReceived
* ApiPartitionReassignment
* [AutoPreferredReplicaLeaderElection](AutoPreferredReplicaLeaderElection.md)
* BrokerChange
* BrokerModifications
* ControlledShutdown
* ControllerChange
* Expire
* IsrChangeNotification
* LeaderAndIsrResponseReceived
* ListPartitionReassignments
* LogDirEventNotification
* MockEvent
* PartitionModifications
* PartitionReassignmentIsrChange
* Reelect
* RegisterBrokerAndReelect
* ReplicaLeaderElection
* ShutdownEventThread
* Startup
* TopicChange
* TopicDeletion
* TopicDeletionStopReplicaResponseReceived
* [TopicUncleanLeaderElectionEnable](TopicUncleanLeaderElectionEnable.md)
* UncleanLeaderElectionEnable
* UpdateFeatures
* UpdateMetadataResponseReceived
* ZkPartitionReassignment

## Review Me

[[implementations]]
.ControllerEvents
[cols="1,1,2",options="header",width="100%"]
|===
| ControllerEvent
| ControllerState
| Description

| <<kafka-controller-ControllerEvent-AutoPreferredReplicaLeaderElection.adoc#, AutoPreferredReplicaLeaderElection>>
| <<kafka-controller-ControllerState.adoc#AutoLeaderBalance, AutoLeaderBalance>>
| [[AutoPreferredReplicaLeaderElection]]

| <<kafka-controller-ControllerEvent-BrokerChange.adoc#, BrokerChange>>
| <<kafka-controller-ControllerState.adoc#BrokerChange, BrokerChange>>
| [[BrokerChange]]

| BrokerModifications
| <<kafka-controller-ControllerState.adoc#BrokerChange, BrokerChange>>
| [[BrokerModifications]]

| ControlledShutdown
| <<kafka-controller-ControllerState.adoc#ControlledShutdown, ControlledShutdown>>
| [[ControlledShutdown]]

| ControllerChange
| <<kafka-controller-ControllerState.adoc#ControllerChange, ControllerChange>>
| [[ControllerChange]]

| Expire
| <<kafka-controller-ControllerState.adoc#ControllerChange, ControllerChange>>
| [[Expire]]

| IsrChangeNotification
| <<kafka-controller-ControllerState.adoc#IsrChange, IsrChange>>
a| [[IsrChangeNotification]] Emitted when the <<kafka-controller-KafkaController.adoc#isrChangeNotificationHandler, ZNodeChildChangeHandler>> has been notified about the znode child change

| <<kafka-controller-ControllerEvent-LeaderAndIsrResponseReceived.adoc#, LeaderAndIsrResponseReceived>>
| <<kafka-controller-ControllerState.adoc#LeaderAndIsrResponseReceived, LeaderAndIsrResponseReceived>>
| [[LeaderAndIsrResponseReceived]]

| LogDirEventNotification
| <<kafka-controller-ControllerState.adoc#LogDirChange, LogDirChange>>
| [[LogDirEventNotification]]

| PartitionModifications
| <<kafka-controller-ControllerState.adoc#TopicChange, TopicChange>>
| [[PartitionModifications]] Emitted when one of the <<kafka-controller-KafkaController.adoc#partitionModificationsHandlers, ZNodeChangeHandler>> has been notified about the znode change

| PartitionReassignment
| <<kafka-controller-ControllerState.adoc#PartitionReassignment, PartitionReassignment>>
| [[PartitionReassignment]]

| PartitionReassignmentIsrChange
| <<kafka-controller-ControllerState.adoc#PartitionReassignment, PartitionReassignment>>
| [[PartitionReassignmentIsrChange]]

| <<kafka-controller-ControllerEvent-PreferredReplicaLeaderElection.adoc#, PreferredReplicaLeaderElection>>
| <<kafka-controller-ControllerState.adoc#ManualLeaderBalance, ManualLeaderBalance>>
| [[PreferredReplicaLeaderElection]]

| <<kafka-controller-ControllerEvent-Reelect.adoc#, Reelect>>
| <<kafka-controller-ControllerState.adoc#ControllerChange, ControllerChange>>
| [[Reelect]]

| RegisterBrokerAndReelect
| <<kafka-controller-ControllerState.adoc#ControllerChange, ControllerChange>>
| [[RegisterBrokerAndReelect]]

| ShutdownEventThread
| <<kafka-controller-ControllerState.adoc#ControllerShutdown, ControllerShutdown>>
| [[ShutdownEventThread]]

| <<kafka-controller-ControllerEvent-Startup.adoc#, Startup>>
| <<kafka-controller-ControllerState.adoc#ControllerChange, ControllerChange>>
| [[Startup]]

| TopicChange
| <<kafka-controller-ControllerState.adoc#TopicChange, TopicChange>>
| [[TopicChange]]

| <<kafka-controller-ControllerEvent-TopicDeletion.adoc#, TopicDeletion>>
| <<kafka-controller-ControllerState.adoc#TopicDeletion, TopicDeletion>>
| [[TopicDeletion]]

| TopicDeletionStopReplicaResponseReceived
| <<kafka-controller-ControllerState.adoc#TopicDeletion, TopicDeletion>>
| [[TopicDeletionStopReplicaResponseReceived]]

| TopicUncleanLeaderElectionEnable
| <<kafka-controller-ControllerState.adoc#TopicUncleanLeaderElectionEnable, TopicUncleanLeaderElectionEnable>>
| [[TopicUncleanLeaderElectionEnable]]

| UncleanLeaderElectionEnable
| <<kafka-controller-ControllerState.adoc#UncleanLeaderElectionEnable, UncleanLeaderElectionEnable>>
| [[UncleanLeaderElectionEnable]] Emitted when `KafkaController` is requested to <<kafka-controller-KafkaController.adoc#enableDefaultUncleanLeaderElection, enableDefaultUncleanLeaderElection>>

|===

NOTE: `ControllerEvent` is a Scala sealed trait and so all the possible <<implementations, controller events>> are in a single compilation unit (i.e. a file).
