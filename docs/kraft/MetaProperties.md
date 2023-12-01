# MetaProperties

`MetaProperties` is [serialized](#toProperties) to `meta.properties` file in [metadata.log.dir](../KafkaConfig.md#metadata.log.dir) (if specified) or the head of the [logDirs](../KafkaConfig.md#logDirs).

`MetaProperties` is used to create a [KafkaRaftManager](KafkaRaftManager.md#metaProperties).

## Creating Instance

`MetaProperties` takes the following to be created:

* <span id="clusterId"> Cluster ID
* <span id="nodeId"> Node ID

`MetaProperties` is created when:

* `KafkaServer` is requested to [startup](../broker/KafkaServer.md#startup) (to create a [KafkaRaftManager](KafkaRaftManager.md#metaProperties))
* `MetaProperties` is requested to [parse](#parse)
