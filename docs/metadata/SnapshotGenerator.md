# SnapshotGenerator

`SnapshotGenerator` is a [MetadataPublisher](MetadataPublisher.md).

## Creating Instance

`SnapshotGenerator` takes the following to be created:

* <span id="nodeId"> Node ID
* <span id="time"> `Time`
* <span id="emitter"> `Emitter`
* <span id="faultHandler"> `FaultHandler`
* <span id="maxBytesSinceLastSnapshot"> [metadata.log.max.record.bytes.between.snapshots](../KafkaConfig.md#metadata.log.max.record.bytes.between.snapshots)
* <span id="maxTimeSinceLastSnapshotNs"> [metadata.log.max.snapshot.interval.ms](../KafkaConfig.md#metadata.log.max.snapshot.interval.ms)
* <span id="disabledReason"> `disabledReason`
* <span id="threadNamePrefix"> Thread Name Prefix

`SnapshotGenerator` is created when:

* `SnapshotGenerator.Builder` is requested to `build` (when `SharedServer` is requested to [start](../kraft/SharedServer.md#snapshotGenerator))

## onMetadataUpdate { #onMetadataUpdate }

??? note "MetadataPublisher"

    ```java
    void onMetadataUpdate(
      MetadataDelta delta,
      MetadataImage newImage,
      LoaderManifest manifest)
    ```

    `onMetadataUpdate` is part of the [MetadataPublisher](MetadataPublisher.md#onMetadataUpdate) abstraction.

`onMetadataUpdate`...FIXME
