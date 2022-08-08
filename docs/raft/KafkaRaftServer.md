# KafkaRaftServer

`KafkaRaftServer` is a [Kafka broker](../Server.md) that runs without Zookeeper.

## Creating Instance

`KafkaRaftServer` takes the following to be created:

* <span id="config"> [KafkaConfig](../KafkaConfig.md)
* <span id="time"> `Time`
* <span id="threadNamePrefix"> Optional `threadNamePrefix`

`KafkaRaftServer` is created when:

* `Kafka` utility is used to [build a Server](../Kafka.md#buildServer) (with no [processRoles](../KafkaConfig.md#processRoles) and hence no Zookeeper)

## <span id="startup"> startup

```scala
startup(): Unit
```

`startup`...FIXME

`startup` is part of the [Server](../Server.md#startup) abstraction.

## <span id="initializeLogDirs"> initializeLogDirs

```scala
initializeLogDirs(
  config: KafkaConfig): (MetaProperties, Seq[String])
```

`initializeLogDirs` uses the [log.dirs (if defined) or log.dir](../KafkaConfig.md#logDirs) and [metadata.log.dir](../KafkaConfig.md#metadataLogDir) for the log directories (`logDirs`).

`initializeLogDirs` [getBrokerMetadataAndOfflineDirs](../BrokerMetadataCheckpoint.md#getBrokerMetadataAndOfflineDirs) the log directories (into `rawMetaProperties` and `offlineDirs`).

!!! note
    [metadata.log.dir](../KafkaConfig.md#metadataLogDir) is not allowed to be among the offline directories (`offlineDirs`) or a `KafkaException` is thrown:

    ```text
    Cannot start server since `meta.properties` could not be loaded from [metadataLogDir]
    ```

`initializeLogDirs`...FIXME

---

`initializeLogDirs` is used when:

* `KafkaRaftServer` is [created](#creating-instance) (and initializes [metaProps](#metaProps) and [offlineDirs](#offlineDirs))

## <span id="offlineDirs"> offlineDirs

```scala
offlineDirs: Seq[String]
```

`KafkaRaftServer` [initializes](#initializeLogDirs) an `offlineDirs` registry when [created](#creating-instance).

`offlineDirs` is used when:

* `KafkaRaftServer` is requested for the [broker](#broker) (to create a [BrokerServer](BrokerServer.md))

## <span id="metaProps"> metaProps

```scala
metaProps: MetaProperties
```

`KafkaRaftServer` [creates a MetaProperties](#initializeLogDirs) when [created](#creating-instance).

`metaProps` is used to initialize the other `KafkaRaftServer` services:

* [BrokerServer](#broker)
* [ControllerServer](#controller)
* [KafkaRaftManager](#raftManager)
* [Metrics](#metrics)

## <span id="broker"> BrokerServer

```scala
broker: Option[BrokerServer]
```

`KafkaRaftServer` creates a [BrokerServer](BrokerServer.md) when [created](#creating-instance) with [BrokerRole](../KafkaConfig.md#processRoles).

The lifecycle of `BrokerServer` is tied up to `KafkaRaftServer`:

* [startup](BrokerServer.md#startup) when `KafkaRaftServer` is requested to [startup](#startup)
* [shutdown](BrokerServer.md#shutdown) when `KafkaRaftServer` is requested to [shutdown](#shutdown)
* [awaitShutdown](BrokerServer.md#awaitShutdown) when `KafkaRaftServer` is requested to [awaitShutdown](#awaitShutdown)
