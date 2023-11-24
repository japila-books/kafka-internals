# RaftManager

`RaftManager` is an [abstraction](#contract) of [Raft managers](#implementations).

## Contract

### client { #client }

```scala
client: RaftClient[T]
```

[RaftClient](RaftClient.md)

See:

* [KafkaRaftManager](KafkaRaftManager.md#client)

### leaderAndEpoch { #leaderAndEpoch }

```scala
leaderAndEpoch: LeaderAndEpoch
```

See:

* [KafkaRaftManager](KafkaRaftManager.md#leaderAndEpoch)

Used when:

* `RaftControllerNodeProvider` is requested to [getControllerInfo](../broker/RaftControllerNodeProvider.md#getControllerInfo)

## Implementations

* [KafkaRaftManager](KafkaRaftManager.md)
