# AbstractControllerBrokerRequestBatch

`AbstractControllerBrokerRequestBatch` is an [abstraction](#contract) of [ControllerBrokerRequestBatches](#implementations) that can [send controller requests to brokers](#sendRequestsToBrokers).

## Contract

### <span id="sendEvent"> Sending ControllerEvent

```scala
sendEvent(
  event: ControllerEvent): Unit
```

Sends the [ControllerEvent](ControllerEvent.md)

Used when:

* `AbstractControllerBrokerRequestBatch` is requested to [sendLeaderAndIsrRequest](#sendLeaderAndIsrRequest), [sendUpdateMetadataRequests](#sendUpdateMetadataRequests), [sendStopReplicaRequests](#sendStopReplicaRequests)

### <span id="sendRequest"> Sending Request (to Broker)

```scala
sendRequest(
  brokerId: Int,
  request: AbstractControlRequest.Builder[_ <: AbstractControlRequest],
  callback: AbstractResponse => Unit = null): Unit
```

Sends an `AbstractControlRequest` out to the given broker (with optional callback to handle a response)

Used when:

* `AbstractControllerBrokerRequestBatch` is requested to [sendLeaderAndIsrRequest](#sendLeaderAndIsrRequest), [sendUpdateMetadataRequests](#sendUpdateMetadataRequests), [sendStopReplicaRequests](#sendStopReplicaRequests), [sendStopReplicaRequests](#sendStopReplicaRequests)

## Implementations

* [ControllerBrokerRequestBatch](ControllerBrokerRequestBatch.md)

## Creating Instance

`AbstractControllerBrokerRequestBatch` takes the following to be created:

* <span id="config"> [KafkaConfig](../KafkaConfig.md)
* <span id="controllerContext"> [ControllerContext](ControllerContext.md)
* <span id="stateChangeLogger"> `StateChangeLogger`

!!! note "Abstract Class"
    `AbstractControllerBrokerRequestBatch` is an abstract class and cannot be created directly. It is created indirectly for the [concrete AbstractControllerBrokerRequestBatches](#implementations).

## <span id="newBatch"> Preparing New Batch

```scala
newBatch(): Unit
```

`newBatch` throws an `IllegalStateException` when any of the internal registries are not empty: 

* [leaderAndIsrRequestMap](#leaderAndIsrRequestMap)

    ```text
    Controller to broker state change requests batch is not empty while creating a new one.
    Some LeaderAndIsr state changes [leaderAndIsrRequestMap] might be lost
    ```

* [stopReplicaRequestMap](#stopReplicaRequestMap)

    ```text
    Controller to broker state change requests batch is not empty while creating a new one.
    Some StopReplica state changes [stopReplicaRequestMap] might be lost
    ```

* [updateMetadataRequestBrokerSet](#updateMetadataRequestBrokerSet)

    ```text
    Controller to broker state change requests batch is not empty while creating a new one.
    Some UpdateMetadata state changes to brokers [updateMetadataRequestBrokerSet] with partition info [updateMetadataRequestPartitionInfoMap] might be lost
    ```

---

`newBatch` is used when:

* `KafkaController` is requested to [updateLeaderEpochAndSendRequest](KafkaController.md#updateLeaderEpochAndSendRequest), [sendUpdateMetadataRequest](KafkaController.md#sendUpdateMetadataRequest), [doControlledShutdown](KafkaController.md#doControlledShutdown)
* `ZkPartitionStateMachine` is requested to [handleStateChanges](ZkPartitionStateMachine.md#handleStateChanges)
* `ZkReplicaStateMachine` is requested to [handleStateChanges](ZkReplicaStateMachine.md#handleStateChanges)

## <span id="sendRequestsToBrokers"> Sending Controller Requests To Brokers

```scala
sendRequestsToBrokers(
  controllerEpoch: Int): Unit
```

`sendRequestsToBrokers` [sends LeaderAndIsr requests out to brokers](#sendLeaderAndIsrRequest).

`sendRequestsToBrokers` [sends UpdateMetadata requests out to brokers](#sendUpdateMetadataRequests).

`sendRequestsToBrokers` [sends StopReplica requests out to brokers](#sendStopReplicaRequests).

---

`sendRequestsToBrokers` is used when:

* `KafkaController` is requested to [updateLeaderEpochAndSendRequest](KafkaController.md#updateLeaderEpochAndSendRequest), [sendUpdateMetadataRequest](KafkaController.md#sendUpdateMetadataRequest), [doControlledShutdown](KafkaController.md#doControlledShutdown)
* `ZkPartitionStateMachine` is requested to [handleStateChanges](ZkPartitionStateMachine.md#handleStateChanges)
* `ZkReplicaStateMachine` is requested to [handleStateChanges](ZkReplicaStateMachine.md#handleStateChanges)

### <span id="sendUpdateMetadataRequests"> Sending Out UpdateMetadata Requests

```scala
sendUpdateMetadataRequests(
  controllerEpoch: Int,
  stateChangeLog: StateChangeLogger): Unit
```

`sendUpdateMetadataRequests` prints out the following INFO message to the logs:

```text
Sending UpdateMetadata request to brokers [updateMetadataRequestBrokerSet] for [size] partitions
```

`sendUpdateMetadataRequests`...FIXME

## <span id="addLeaderAndIsrRequestForBrokers"> addLeaderAndIsrRequestForBrokers

```scala
addLeaderAndIsrRequestForBrokers(
  brokerIds: Seq[Int],
  topicPartition: TopicPartition,
  leaderIsrAndControllerEpoch: LeaderIsrAndControllerEpoch,
  replicaAssignment: ReplicaAssignment,
  isNew: Boolean): Unit
```

`addLeaderAndIsrRequestForBrokers`...FIXME

---

`addLeaderAndIsrRequestForBrokers` is used when:

* `KafkaController` is requested to [updateLeaderEpochAndSendRequest](KafkaController.md#updateLeaderEpochAndSendRequest)
* `ZkPartitionStateMachine` is requested to [initializeLeaderAndIsrForPartitions](ZkPartitionStateMachine.md#initializeLeaderAndIsrForPartitions) and [doElectLeaderForPartitions](ZkPartitionStateMachine.md#doElectLeaderForPartitions)
* `ZkReplicaStateMachine` is requested to [doHandleStateChanges](ZkReplicaStateMachine.md#doHandleStateChanges)
