# RaftClient

`RaftClient` is an [abstraction](#contract) of [Raft clients](#implementations).

`RaftClient` is `AutoCloseable`.

## Contract

### initialize { #initialize }

```java
void initialize()
```

See:

* [KafkaRaftClient](KafkaRaftClient.md#initialize)

Used when:

* `KafkaRaftManager` is requested to [build a RaftClient](KafkaRaftManager.md#buildRaftClient)

## Implementations

* [KafkaRaftClient](KafkaRaftClient.md)
