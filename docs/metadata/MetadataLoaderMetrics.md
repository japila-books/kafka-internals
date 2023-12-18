# MetadataLoaderMetrics

## Creating Instance

`MetadataLoaderMetrics` takes the following to be created:

* <span id="registry"> `MetricsRegistry`
* <span id="batchProcessingTimeNsUpdater"> `batchProcessingTimeNsUpdater`
* <span id="batchSizesUpdater"> `batchSizesUpdater`
* <span id="lastAppliedProvenance"> Last Applied `MetadataProvenance`

`MetadataLoaderMetrics` is created when:

* `SharedServer` is requested to [start](../kraft/SharedServer.md#start)
* `MetadataLoader.Builder` is requested to `build`
