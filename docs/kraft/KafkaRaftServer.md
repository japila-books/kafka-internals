# KafkaRaftServer

`KafkaRaftServer` is a [Server](../Server.md) for [KRaft mode](index.md).

## Creating Instance

`KafkaRaftServer` takes the following to be created:

* <span id="config"> [KafkaConfig](../KafkaConfig.md)
* <span id="time"> `Time`
* <span id="threadNamePrefix"> Thread Name Prefix (optional)

`KafkaRaftServer` is created when:

* `Kafka` command-line application is [launched](../Kafka.md#main) (and [builds a server](../Kafka.md#buildServer) with no [process.roles](../KafkaConfig.md#processRoles))

## startup { #startup }

??? note "Server"

    ```scala
    startup(): Unit
    ```

    `startup` is part of the [Server](../Server.md#startup) abstraction.

`startup`...FIXME

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

## BrokerServer { #broker }

```scala
broker: Option[BrokerServer]
```

`KafkaRaftServer` creates a [BrokerServer](BrokerServer.md) when [created](#creating-instance) with [BrokerRole](../KafkaConfig.md#processRoles).

The lifecycle of `BrokerServer` is tied up to `KafkaRaftServer`:

* [startup](BrokerServer.md#startup) when `KafkaRaftServer` is requested to [startup](#startup)
* [shutdown](BrokerServer.md#shutdown) when `KafkaRaftServer` is requested to [shutdown](#shutdown)
* [awaitShutdown](BrokerServer.md#awaitShutdown) when `KafkaRaftServer` is requested to [awaitShutdown](#awaitShutdown)

## ControllerServer { #controller }

```scala
controller: Option[ControllerServer]
```

`KafkaRaftServer` creates a [ControllerServer](ControllerServer.md) when [created](#creating-instance) with [ControllerRole](../KafkaConfig.md#processRoles).

The lifecycle of `ControllerServer` is tied up to `KafkaRaftServer`:

* [startup](ControllerServer.md#startup) when `KafkaRaftServer` is requested to [startup](#startup)
* [shutdown](ControllerServer.md#shutdown) when `KafkaRaftServer` is requested to [shutdown](#shutdown)
* [awaitShutdown](ControllerServer.md#awaitShutdown) when `KafkaRaftServer` is requested to [awaitShutdown](#awaitShutdown)

## SharedServer { #sharedServer }

`KafkaRaftServer` creates a [SharedServer](SharedServer.md) when [created](#creating-instance).

The `SharedServer` is used when running in [KRaft mode](index.md) to create the following:

* [BrokerServer](#broker) when configured as a [broker](../KafkaConfig.md#processRoles)
* [ControllerServer](#controller) when configured as a [controller](../KafkaConfig.md#processRoles)
