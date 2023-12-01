# KafkaRaftManager

`KafkaRaftManager` is a [RaftManager](RaftManager.md).

## Creating Instance

`KafkaRaftManager` takes the following to be created:

* <span id="metaProperties"> [MetaProperties](MetaProperties.md)
* <span id="config"> [KafkaConfig](../KafkaConfig.md)
* <span id="recordSerde"> `RecordSerde[T]`
* [Partition 0 of __cluster_metadata](#topicPartition)
* <span id="topicId"> Topic ID (UUID)
* <span id="time"> `Time`
* <span id="metrics"> [Metrics](../metrics/Metrics.md)
* <span id="threadNamePrefixOpt"> Thread Name Prefix (optional)
* <span id="controllerQuorumVotersFuture"> Controller Quorum Voters (`CompletableFuture[util.Map[Integer, AddressSpec]]`)
* <span id="fatalFaultHandler"> `FaultHandler`

`KafkaRaftManager` is created when:

* `KafkaServer` is requested to [start up](../broker/KafkaServer.md#startup) (with [zookeeper.metadata.migration.enable](../KafkaConfig.md#zookeeper.metadata.migration.enable) enabled)
* `SharedServer` is requested to [start](SharedServer.md#start)

### \_\_cluster_metadata-0 Partition { #topicPartition }

`KafkaRaftManager` is given a `TopicPartition` when [created](#creating-instance):

* `__cluster_metadata` as the name of the cluster metadata topic
* Partition `0`

## Metadata Log Directory { #dataDir }

`KafkaRaftManager` [creates a data directory](KafkaRaftManager.md#createDataDir) when [created](#creating-instance).

### createDataDir { #createDataDir }

```scala
createDataDir(): File
```

`createDataDir` [creates the name of the log directory](../log/UnifiedLog.md#logDirName) of the [TopicPartition](#topicPartition).

`createDataDir` [creates the directory](#createLogDirectory) in the [metadataLogDir](../KafkaConfig.md#metadataLogDir).
