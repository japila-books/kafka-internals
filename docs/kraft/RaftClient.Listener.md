# RaftClient.Listener

`RaftClient.Listener<T>` is an [abstraction](#contract) of [listeners](#implementations) that can [handleCommit](#handleCommit) and [handleLoadSnapshot](#handleLoadSnapshot) (_among other metadata-related things_).

## Contract (Subset)

### handleCommit { #handleCommit }

```java
void handleCommit(
  BatchReader<T> reader)
```

Used when:

* `SnapshotFileReader` is requested to `handleMetadataBatch`
* `KafkaRaftClient.ListenerContext` is requested to [fireHandleCommit](KafkaRaftClient.ListenerContext.md#fireHandleCommit)

### handleLoadSnapshot { #handleLoadSnapshot }

```java
void handleLoadSnapshot(
  SnapshotReader<T> reader)
```

Used when:

* `KafkaRaftClient.ListenerContext` is requested to [fireHandleSnapshot](KafkaRaftClient.ListenerContext.md#fireHandleSnapshot)

## Implementations

* [MetadataLoader](../metadata/MetadataLoader.md)
* `OffsetTrackingListener`
* `QuorumMetaLogListener`
* `ReplicatedCounter`
