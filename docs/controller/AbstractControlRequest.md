# AbstractControlRequest

`AbstractControlRequest` is an [extension](#contract) of the [AbstractRequest](../AbstractRequest.md) abstraction for [controller requests](#implementations) that [KafkaController](KafkaController.md) uses to propage broker and partition state changes to brokers.

## Contract

### <span id="brokerEpoch"> brokerEpoch

```java
long brokerEpoch()
```

Used when:

* `KafkaApis` is requested to [handleLeaderAndIsrRequest](../KafkaApis.md#handleLeaderAndIsrRequest), [handleStopReplicaRequest](../KafkaApis.md#handleStopReplicaRequest), [handleUpdateMetadataRequest](../KafkaApis.md#handleUpdateMetadataRequest)

### <span id="controllerEpoch"> controllerEpoch

```java
int controllerEpoch()
```

Used when:

* `KafkaApis` is requested to [handleStopReplicaRequest](../KafkaApis.md#handleStopReplicaRequest)
* `ReplicaManager` is requested to [maybeUpdateMetadataCache](../ReplicaManager.md#maybeUpdateMetadataCache), [becomeLeaderOrFollower](../ReplicaManager.md#becomeLeaderOrFollower)
* `ZkMetadataCache` is requested to `updateMetadata`

### <span id="controllerId"> controllerId

```java
int controllerId()
```

Used when:

* `KafkaApis` is requested to [handleStopReplicaRequest](../KafkaApis.md#handleStopReplicaRequest)
* `ReplicaManager` is requested to [maybeUpdateMetadataCache](../ReplicaManager.md#maybeUpdateMetadataCache), [becomeLeaderOrFollower](../ReplicaManager.md#becomeLeaderOrFollower)
* `ZkMetadataCache` is requested to `updateMetadata`

## Implementations

* [LeaderAndIsrRequest](LeaderAndIsrRequest.md)
* `StopReplicaRequest`
* `UpdateMetadataRequest`

## Creating Instance

`AbstractControlRequest` takes the following to be created:

* <span id="api"> `ApiKeys`
* <span id="version"> Version

!!! note "Abstract Class"
    `AbstractControlRequest` is an abstract class and cannot be created directly. It is created indirectly for the [concrete AbstractControlRequests](#implementations).
