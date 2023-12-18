# MetadataLoader

`MetadataLoader` is built for a [SharedServer](../kraft/SharedServer.md#loader).

## Creating Instance

`MetadataLoader` takes the following to be created:

* <span id="time"> `Time`
* <span id="logContext"> `LogContext`
* <span id="nodeId"> Node ID (_not used_)
* <span id="threadNamePrefix"> Thread Name Prefix
* <span id="faultHandler"> `FaultHandler`
* <span id="metrics"> `MetadataLoaderMetrics`
* [HighWaterMarkAccessor](#highWaterMarkAccessor)

`MetadataLoader` is created when:

* `MetadataLoader.Builder` is requested to [build a MetadataLoader](#build)

### HighWaterMarkAccessor { #highWaterMarkAccessor }

`MetadataLoader` is given a high watermark accessor (a `Supplier<OptionalLong>`) when [created](#creating-instance) (when `SharedServer` is requested to [start](../kraft/SharedServer.md#start)).

The high watermark accessor is the [highWatermark](../kraft/KafkaRaftClient.md#highWatermark) from the [KafkaRaftClient](../kraft/KafkaRaftManager.md#client) of a [KafkaRaftManager](../kraft/KafkaRaftManager.md).

## Installing MetadataPublishers { #installPublishers }

```java
CompletableFuture<Void> installPublishers(
  List<? extends MetadataPublisher> newPublishers)
```

`installPublishers` adds all the [MetadataPublisher](MetadataPublisher.md)s (in the given `newPublishers`) to the [uninitializedPublishers](#uninitializedPublishers) registry followed by [scheduleInitializeNewPublishers](#scheduleInitializeNewPublishers).

---

`installPublishers` is used when:

* `MetadataShell` is requested to [run](../tools/kafka-metadata-shell/MetadataShell.md#run) (to register a [MetadataShellPublisher](../tools/kafka-metadata-shell/MetadataShellPublisher.md))
* `BrokerServer` is requested to [startup](../kraft/BrokerServer.md#startup)
* `ControllerServer` is requested to [startup](../kraft/ControllerServer.md#startup)
* `SharedServer` is requested to [start](../kraft/SharedServer.md#start) (to register a [SnapshotGenerator](SnapshotGenerator.md))

## scheduleInitializeNewPublishers { #scheduleInitializeNewPublishers }

```java
void scheduleInitializeNewPublishers(
  long delayNs)
```

`scheduleInitializeNewPublishers`...FIXME

---

`scheduleInitializeNewPublishers` is used when:

* `MetadataLoader` is requested to [initializeNewPublishers](#initializeNewPublishers), [maybePublishMetadata](#maybePublishMetadata) and [installPublishers](#installPublishers)

### initializeNewPublishers { #initializeNewPublishers }

```java
void initializeNewPublishers()
```

`initializeNewPublishers`...FIXME

## maybePublishMetadata { #maybePublishMetadata }

```java
void maybePublishMetadata(
  MetadataDelta delta,
  MetadataImage image,
  LoaderManifest manifest)
```

`maybePublishMetadata`...FIXME

---

`maybePublishMetadata` is used when:

* `MetadataLoader` is [created](#batchLoader) and requested to [handleLoadSnapshot](#handleLoadSnapshot)

## handleLoadSnapshot { #handleLoadSnapshot }

??? note "RaftClient.Listener"

    ```java
    void handleLoadSnapshot(
      SnapshotReader<ApiMessageAndVersion> reader)
    ```

    `handleLoadSnapshot` is part of the [RaftClient.Listener](../kraft/RaftClient.Listener.md#handleLoadSnapshot) abstraction.

`handleLoadSnapshot`...FIXME

### loadSnapshot { #loadSnapshot }

```java
SnapshotManifest loadSnapshot(
  MetadataDelta delta,
  SnapshotReader<ApiMessageAndVersion> reader)
```

`loadSnapshot`...FIXME
