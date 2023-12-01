# KafkaRaftClient

`KafkaRaftClient` is a [RaftClient](RaftClient.md).

## Creating Instance

`KafkaRaftClient` takes the following to be created:

* <span id="serde"> `RecordSerde<T>`
* <span id="channel"> `NetworkChannel`
* <span id="messageQueue"> `RaftMessageQueue`
* <span id="log"> `ReplicatedLog`
* [QuorumStateStore](#quorumStateStore)
* <span id="memoryPool"> `MemoryPool`
* <span id="time"> `Time`
* <span id="metrics"> [Metrics](../metrics/Metrics.md)
* <span id="expirationService"> `ExpirationService`
* <span id="fetchMaxWaitMs"> `fetchMaxWaitMs` (`500` ms)
* <span id="clusterId"> Cluster ID
* <span id="nodeId"> [Node ID](../KafkaConfig.md#nodeId)
* <span id="logContext"> `LogContext`
* <span id="random"> `Random`
* <span id="raftConfig"> [RaftConfig](RaftConfig.md)

`KafkaRaftClient` is created when:

* `KafkaRaftManager` is requested to [build a KafkaRaftClient](KafkaRaftManager.md#buildRaftClient)

### QuorumStateStore { #quorumStateStore }

`KafkaRaftClient` is given a [QuorumStateStore](QuorumStateStore.md) when [created](#creating-instance).

The `QuorumStateStore` is a [FileBasedStateStore](FileBasedStateStore.md) with the `quorum-state` state file in the [dataDir](KafkaRaftManager.md#dataDir).
