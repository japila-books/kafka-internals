# ControllerEvent

`ControllerEvent` is the <<contract, contract>> of the <<implementations, events>> in the lifecycle of <<kafka-controller-KafkaController.adoc#, KafkaController>> (state machine) that trigger <<state, state>> change.

`ControllerEvent` events are managed by <<kafka-controller-ControllerEventManager.adoc#, ControllerEventManager>>.

[[contract]]
.ControllerEvent Contract
[cols="30m,70",options="header",width="100%"]
|===
| Method
| Description

| state
a| [[state]]

[source, scala]
----
state: ControllerState
----

Requested <<kafka-controller-ControllerState.adoc#, ControllerState>> of the <<kafka-controller-ControllerEventManager.adoc#, ControllerEventManager>> (while <<kafka-controller-ControllerEventThread.adoc#doWork, processing a controller event>>)

|===

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
