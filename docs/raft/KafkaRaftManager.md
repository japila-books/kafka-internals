# KafkaRaftManager

`KafkaRaftManager` is a [RaftManager](RaftManager.md).

## Creating Instance

`KafkaRaftManager` takes the following to be created:

* <span id="metaProperties"> [MetaProperties](MetaProperties.md)
* <span id="config"> [KafkaConfig](../KafkaConfig.md)
* <span id="recordSerde"> `RecordSerde[T]`
* <span id="topicPartition"> `TopicPartition`
* <span id="topicId"> Topic ID (UUID)
* <span id="time"> `Time`
* <span id="metrics"> [Metrics](../metrics/Metrics.md)
* <span id="threadNamePrefixOpt"> Thread Name Prefix (optional)
* <span id="controllerQuorumVotersFuture"> Controller Quorum Voters (`CompletableFuture[util.Map[Integer, AddressSpec]]`)
* <span id="fatalFaultHandler"> `FaultHandler`

`KafkaRaftManager` is created when:

* `KafkaServer` is requested to [start up](../broker/KafkaServer.md#startup) (with [zookeeper.metadata.migration.enable](../KafkaConfig.md#zookeeper.metadata.migration.enable) enabled)
* `SharedServer` is requested to [start](SharedServer.md#start)
