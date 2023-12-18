# SharedServer

## Creating Instance

`SharedServer` takes the following to be created:

* <span id="sharedServerConfig"> [KafkaConfig](../KafkaConfig.md)
* <span id="metaProps"> [MetaProperties](MetaProperties.md)
* <span id="time"> `Time`
* <span id="_metrics"> [Metrics](../metrics/Metrics.md)
* <span id="controllerQuorumVotersFuture"> Controller Quorum Voters (`CompletableFuture[util.Map[Integer, AddressSpec]]`)
* <span id="faultHandlerFactory"> `FaultHandlerFactory`

`SharedServer` is created alongside [KafkaRaftServer](KafkaRaftServer.md#sharedServer).

## SnapshotGenerator { #snapshotGenerator }

`SharedServer` creates a new [SnapshotGenerator](../metadata/SnapshotGenerator.md) at [start up](#start) as follows:

Property | Value
---------|------
 [Emitter](../metadata/SnapshotGenerator.md#emitter) | [SnapshotEmitter](#snapshotEmitter)
 [maxBytesSinceLastSnapshot](../metadata/SnapshotGenerator.md#maxBytesSinceLastSnapshot) | [metadata.log.max.record.bytes.between.snapshots](../KafkaConfig.md#metadata.log.max.record.bytes.between.snapshots)
 [maxTimeSinceLastSnapshotNs](../metadata/SnapshotGenerator.md#maxTimeSinceLastSnapshotNs) | [metadata.log.max.snapshot.interval.ms](../KafkaConfig.md#metadata.log.max.snapshot.interval.ms)
 [threadNamePrefix](../metadata/SnapshotGenerator.md#threadNamePrefix) | `kafka-[nodeId]-`

The `SnapshotGenerator` is closed (and the reference de-referenced, `null`ed) at [stop](#stop).

The `SnapshotGenerator` is used at [start up](#start) for the [MetadataLoader](#loader) to [installPublishers](../metadata/MetadataLoader.md#installPublishers).
