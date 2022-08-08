# KafkaApis

## Creating Instance

`KafkaApis` takes the following to be created:

* <span id="requestChannel"> `RequestChannel`
* <span id="metadataSupport"> `MetadataSupport`
* <span id="replicaManager"> `ReplicaManager`
* <span id="groupCoordinator"> `GroupCoordinator`
* [TransactionCoordinator](#txnCoordinator)
* <span id="autoTopicCreationManager"> [AutoTopicCreationManager](AutoTopicCreationManager.md)
* <span id="brokerId"> Broker ID
* <span id="config"> [KafkaConfig](KafkaConfig.md)
* <span id="configRepository"> `ConfigRepository`
* <span id="metadataCache"> `MetadataCache`
* <span id="metrics"> `Metrics`
* <span id="authorizer"> Optional `Authorizer`
* <span id="quotas"> `QuotaManagers`
* <span id="fetchManager"> `FetchManager`
* <span id="brokerTopicStats"> `BrokerTopicStats`
* <span id="clusterId"> Cluster ID
* <span id="time"> `Time`
* <span id="tokenManager"> `DelegationTokenManager`
* <span id="apiVersionManager"> `ApiVersionManager`

`KafkaApis` is created when:

* `BrokerServer` is requested to [startup](raft/BrokerServer.md#startup) (for the [dataPlaneRequestProcessor](raft/BrokerServer.md#dataPlaneRequestProcessor) and the [controlPlaneRequestProcessor](raft/BrokerServer.md#controlPlaneRequestProcessor))
* `KafkaServer` is requested to [startup](broker/KafkaServer.md#startup) (for the [dataPlaneRequestProcessor](broker/KafkaServer.md#dataPlaneRequestProcessor) and the [controlPlaneRequestProcessor](broker/KafkaServer.md#controlPlaneRequestProcessor))

## <span id="txnCoordinator"> TransactionCoordinator

`KafkaApis` is given a [TransactionCoordinator](transactions/TransactionCoordinator.md) when [created](#creating-instance).

The `TransactionCoordinator` is used for the following:

* [handleAddOffsetsToTxnRequest](#handleAddOffsetsToTxnRequest)
* [handleAddPartitionToTxnRequest](#handleAddPartitionToTxnRequest)
* [handleEndTxnRequest](#handleEndTxnRequest)
* [handleFindCoordinatorRequest](#handleFindCoordinatorRequest)
* [handleInitProducerIdRequest](#handleInitProducerIdRequest)
* [handleLeaderAndIsrRequest](#handleLeaderAndIsrRequest)
* [handleStopReplicaRequest](#handleStopReplicaRequest)

## <span id="handleInitProducerIdRequest"> handleInitProducerIdRequest

```scala
handleInitProducerIdRequest(
  request: RequestChannel.Request): Unit
```

`handleInitProducerIdRequest` assumes that the given `RequestChannel.Request` is an `InitProducerIdRequest`.

`handleInitProducerIdRequest` authorizes the request.

With `producerId` and `producerEpoch` set either to `-1`s (`NO_PRODUCER_ID` and `NO_PRODUCER_EPOCH`) or some non-`-1` values, `handleInitProducerIdRequest` requests the [TransactionCoordinator](#txnCoordinator) to [handleInitProducerId](transactions/TransactionCoordinator.md#handleInitProducerId).

Otherwise, `handleInitProducerIdRequest` sends an error back.

`handleInitProducerIdRequest` is used when:

* `KafkaApis` is requested to [handle a INIT_PRODUCER_ID request](#handle)

## <span id="handleFetchRequest"> handleFetchRequest

```scala
handleFetchRequest(
  request: RequestChannel.Request): Unit
```

`handleFetchRequest` assumes that the given `RequestChannel.Request` is an `FetchRequest`.

`handleFetchRequest` authorizes the request.

In the end, `handleFetchRequest` requests the [ReplicaManager](#replicaManager) to [fetchMessages](ReplicaManager.md#fetchMessages).

`handleFetchRequest` is used when:

* `KafkaApis` is requested to [handle a FETCH request](#handle)
