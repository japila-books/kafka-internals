# AbstractControllerBrokerRequestBatch

## Review Me

`AbstractControllerBrokerRequestBatch` is the <<contract, base>> of <<extensions, ControllerBrokerRequestBatches>> that <<sendRequestsToBrokers, sendRequestsToBrokers>>.

[[implementations]]
NOTE: <<kafka-controller-ControllerBrokerRequestBatch.adoc#, ControllerBrokerRequestBatch>> is the default and only known implementation of the <<contract, AbstractControllerBrokerRequestBatch Contract>> in Apache Kafka.

[[contract]]
.AbstractControllerBrokerRequestBatch Contract (Abstract Methods Only)
[cols="30m,70",options="header",width="100%"]
|===
| Method
| Description

| sendEvent
a| [[sendEvent]]

[source, scala]
----
sendEvent(
  event: ControllerEvent): Unit
----

Sends a <<kafka-controller-ControllerEvent.adoc#, ControllerEvent>>

Used when `AbstractControllerBrokerRequestBatch` is requested to <<sendLeaderAndIsrRequest, sendLeaderAndIsrRequest>> and <<sendStopReplicaRequests, sendStopReplicaRequests>>

| sendRequest
a| [[sendRequest]]

[source, scala]
----
sendRequest(
  brokerId: Int,
  request: AbstractControlRequest.Builder[_ <: AbstractControlRequest],
  callback: AbstractResponse => Unit = null): Unit
----

Sends an <<kafka-controller-AbstractControlRequest.adoc#, controller request>> out to the given broker with optional callback to handle a response

Used when `AbstractControllerBrokerRequestBatch` is requested to <<sendLeaderAndIsrRequest, sendLeaderAndIsrRequest>>, <<sendUpdateMetadataRequests, sendUpdateMetadataRequests>>, and <<sendStopReplicaRequests, sendStopReplicaRequests>>

|===

=== [[creating-instance]] Creating AbstractControllerBrokerRequestBatch Instance

`AbstractControllerBrokerRequestBatch` takes the following to be created:

* [[config]] <<kafka-server-KafkaConfig.adoc#, KafkaConfig>>
* [[controllerContext]] <<kafka-controller-ControllerContext.adoc#, ControllerContext>>
* [[stateChangeLogger]] link:kafka-controller-StateChangeLogger.adoc[StateChangeLogger]

`AbstractControllerBrokerRequestBatch` initializes the <<internal-properties, internal properties>>.

NOTE: `AbstractControllerBrokerRequestBatch` is a Scala abstract class and cannot be <<creating-instance, created>> directly. It is created indirectly for the <<implementations, concrete AbstractControllerBrokerRequestBatches>>.

=== [[sendRequestsToBrokers]] Sending Controller Requests To Brokers -- `sendRequestsToBrokers` Method

[source, scala]
----
sendRequestsToBrokers(
  controllerEpoch: Int): Unit
----

`sendRequestsToBrokers` <<sendLeaderAndIsrRequest, sends LeaderAndIsr requests out to brokers>>.

`sendRequestsToBrokers` <<sendUpdateMetadataRequests, sends UpdateMetadata requests out to brokers>>.

`sendRequestsToBrokers` <<sendStopReplicaRequests, sends StopReplica requests out to brokers>>.

In case of any error (`Throwable`), `sendRequestsToBrokers`...FIXME

[NOTE]
====
`sendRequestsToBrokers` is used when:

* `KafkaController` is requested to <<kafka-controller-KafkaController.adoc#updateLeaderEpochAndSendRequest, updateLeaderEpochAndSendRequest>>, <<kafka-controller-KafkaController.adoc#sendUpdateMetadataRequest, sendUpdateMetadataRequest>> and at <<kafka-controller-KafkaController.adoc#ControlledShutdown, ControlledShutdown>> controller event

* `ZkPartitionStateMachine` is requested to <<kafka-controller-ZkPartitionStateMachine.adoc#handleStateChanges, handleStateChanges>>

* `ReplicaStateMachine` is requested to <<kafka-controller-ReplicaStateMachine.adoc#handleStateChanges, handleStateChanges>>
====

==== [[sendLeaderAndIsrRequest]] Sending LeaderAndIsr Requests Out to All Brokers -- `sendLeaderAndIsrRequest` Internal Method

[source, scala]
----
sendLeaderAndIsrRequest(
  controllerEpoch: Int,
  stateChangeLog: StateChangeLogger): Unit
----

`sendLeaderAndIsrRequest`...FIXME

NOTE: `sendLeaderAndIsrRequest` is used exclusively when `AbstractControllerBrokerRequestBatch` is requested to <<sendRequestsToBrokers, sendRequestsToBrokers>>.

==== [[sendUpdateMetadataRequests]] Sending UpdateMetadata Requests Out to All Brokers -- `sendUpdateMetadataRequests` Internal Method

[source, scala]
----
sendUpdateMetadataRequests(
  controllerEpoch: Int,
  stateChangeLog: StateChangeLogger): Unit
----

`sendUpdateMetadataRequests` prints out the following TRACE message to the logs for every pair in the <<updateMetadataRequestPartitionInfoMap, updateMetadataRequestPartitionInfoMap>> internal registry:

```
Sending UpdateMetadata request [partitionState] to brokers [updateMetadataRequestBrokerSet] for partition [tp]
```

`sendUpdateMetadataRequests` prepares metadata for <<kafka-common-requests-UpdateMetadataRequest.adoc#, UpdateMetadataRequests>>.

For every broker (in the <<updateMetadataRequestBrokerSet, updateMetadataRequestBrokerSet>> internal registry) that is <<kafka-controller-ControllerContext.adoc#liveOrShuttingDownBrokerIds, live or shutting down>> `sendUpdateMetadataRequests` requests the <<controllerContext, ControllerContext>> for the <<kafka-controller-ControllerContext.adoc#liveBrokerIdAndEpochs, broker epoch>>, creates a new <<kafka-common-requests-UpdateMetadataRequest.adoc#Builder, UpdateMetadataRequest>> (with the <<updateMetadataRequestPartitionInfoMap, updateMetadataRequestPartitionInfoMap>> among others) and then <<sendRequest, sends the request out to the broker>>.

In the end, `sendUpdateMetadataRequests` removes all elements from (_clears_) the <<updateMetadataRequestBrokerSet, updateMetadataRequestBrokerSet>> and <<updateMetadataRequestPartitionInfoMap, updateMetadataRequestPartitionInfoMap>> internal registries.

NOTE: `sendUpdateMetadataRequests` is used exclusively when `AbstractControllerBrokerRequestBatch` is requested to <<sendRequestsToBrokers, send controller requests to all brokers in a cluster>>.

==== [[sendStopReplicaRequests]] Sending StopReplica Requests Out to All Brokers -- `sendStopReplicaRequests` Internal Method

[source, scala]
----
sendStopReplicaRequests(
  controllerEpoch: Int): Unit
----

`sendStopReplicaRequests`...FIXME

NOTE: `sendStopReplicaRequests` is used exclusively when `AbstractControllerBrokerRequestBatch` is requested to <<sendRequestsToBrokers, sendRequestsToBrokers>>.

=== [[newBatch]] Preparing New Batch -- `newBatch` Method

[source, scala]
----
newBatch(): Unit
----

`newBatch` simply throws an `IllegalStateException` when any of the following internal registries are not empty: <<leaderAndIsrRequestMap, leaderAndIsrRequestMap>>, <<stopReplicaRequestMap, stopReplicaRequestMap>> and <<updateMetadataRequestBrokerSet, updateMetadataRequestBrokerSet>>.

```
Controller to broker state change requests batch is not empty while creating a new one. Some LeaderAndIsr state changes [leaderAndIsrRequestMap] might be lost
```

```
Controller to broker state change requests batch is not empty while creating a new one. Some StopReplica state changes [stopReplicaRequestMap] might be lost
```

```
Controller to broker state change requests batch is not empty while creating a new one. Some UpdateMetadata state changes to brokers [updateMetadataRequestBrokerSet] with partition info [updateMetadataRequestPartitionInfoMap] might be lost
```

[NOTE]
====
`newBatch` is used when:

* `KafkaController` is requested to <<kafka-controller-KafkaController.adoc#updateLeaderEpochAndSendRequest, updateLeaderEpochAndSendRequest>>, <<kafka-controller-KafkaController.adoc#sendUpdateMetadataRequest, sendUpdateMetadataRequest>>, and process a <<kafka-controller-KafkaController.adoc#ControlledShutdown, ControlledShutdown>> controller event

* `PartitionStateMachine` is requested to <<kafka-controller-PartitionStateMachine.adoc#handleStateChanges, handleStateChanges>>

* `PartitionStateMachine` is requested to <<kafka-controller-ReplicaStateMachine.adoc#handleStateChanges, handleStateChanges>>
====

=== [[addLeaderAndIsrRequestForBrokers]] `addLeaderAndIsrRequestForBrokers` Method

[source, scala]
----
addLeaderAndIsrRequestForBrokers(
  brokerIds: Seq[Int],
  topicPartition: TopicPartition,
  leaderIsrAndControllerEpoch: LeaderIsrAndControllerEpoch,
  replicas: Seq[Int],
  isNew: Boolean): Unit
----

`addLeaderAndIsrRequestForBrokers` looks up the `LeaderAndIsrPartitionStates` of `TopicPartitions` on a broker in the <<leaderAndIsrRequestMap, leaderAndIsrRequestMap>> internal registry. `addLeaderAndIsrRequestForBrokers` uses IDs from the given `brokerIds` that are `0` or higher.

`addLeaderAndIsrRequestForBrokers` adds (or replaces) the given `topicPartition` in the result with a new `LeaderAndIsrPartitionState` based on the input arguments.

In the end, `addLeaderAndIsrRequestForBrokers` <<addUpdateMetadataRequestForBrokers, addUpdateMetadataRequestForBrokers>> to the link:kafka-controller-ControllerContext.adoc#liveOrShuttingDownBrokerIds[live or shutting down brokers] for the requested partition (`topicPartition`).

[NOTE]
====
`addLeaderAndIsrRequestForBrokers` is used when:

* `KafkaController` is requested to link:kafka-controller-KafkaController.adoc#updateLeaderEpochAndSendRequest[updateLeaderEpochAndSendRequest]

* `ZkPartitionStateMachine` is requested to link:kafka-controller-ZkPartitionStateMachine.adoc#initializeLeaderAndIsrForPartitions[initializeLeaderAndIsrForPartitions] and link:kafka-controller-ZkPartitionStateMachine.adoc#doElectLeaderForPartitions[doElectLeaderForPartitions]

* `ZkReplicaStateMachine` is requested to link:kafka-controller-ZkReplicaStateMachine.adoc#doHandleStateChanges[\]
====

=== [[addUpdateMetadataRequestForBrokers]] `addUpdateMetadataRequestForBrokers` Method

[source, scala]
----
addUpdateMetadataRequestForBrokers(
  brokerIds: Seq[Int],
  partitions: collection.Set[TopicPartition]): Unit
----

`addUpdateMetadataRequestForBrokers`...FIXME

[NOTE]
====
`addUpdateMetadataRequestForBrokers` is used when:

* `AbstractControllerBrokerRequestBatch` is requested to <<addLeaderAndIsrRequestForBrokers, addLeaderAndIsrRequestForBrokers>>

* `KafkaController` is requested to link:kafka-controller-KafkaController.adoc#sendUpdateMetadataRequest[sendUpdateMetadataRequest]

* `ZkReplicaStateMachine` is requested to link:kafka-controller-ZkReplicaStateMachine.adoc#doHandleStateChanges[doHandleStateChanges]
====

=== [[internal-properties]] Internal Properties

[cols="30m,70",options="header",width="100%"]
|===
| Name
| Description

| leaderAndIsrRequestMap
a| [[leaderAndIsrRequestMap]] `LeaderAndIsrPartitionState` of `TopicPartitions` by broker ID

[source, scala]
----
leaderAndIsrRequestMap: Map[
  Int,
  Map[TopicPartition, LeaderAndIsrRequestData.LeaderAndIsrPartitionState]]
----

Used when...FIXME

| stopReplicaRequestMap
a| [[stopReplicaRequestMap]] (`Map[Int, ListBuffer[StopReplicaRequestInfo]]`)

Used when...FIXME

| updateMetadataRequestBrokerSet
a| [[updateMetadataRequestBrokerSet]] Broker IDs to <<sendUpdateMetadataRequests, send UpdateMetadata requests out to>>

* Broker IDs added in <<addUpdateMetadataRequestForBrokers, addUpdateMetadataRequestForBrokers>>

* Cleared (_emptied_) in <<clear, clear>> and after successful <<sendUpdateMetadataRequests, sendUpdateMetadataRequests>>

|===
