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
