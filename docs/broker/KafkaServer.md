# KafkaServer

`KafkaServer` is a [Server](../Server.md) for Zookeeper mode (non-[KRaft mode](../raft/index.md)).

## Creating Instance

`KafkaServer` takes the following to be created:

* <span id="config"> [KafkaConfig](../KafkaConfig.md)
* <span id="time"> `Time` (default: `SYSTEM`)
* <span id="threadNamePrefix"> Optional Thread Name Prefix (default: undefined)
* [enableForwarding](#enableForwarding) flag

`KafkaServer` is created when:

* `Kafka` command-line application is [launched](../Kafka.md#main) (and [builds a server](../Kafka.md#buildServer) with [process.roles](../KafkaConfig.md#processRoles) specified)

### <span id="enableForwarding"> enableForwarding

```scala
enableForwarding: Boolean
```

`KafkaServer` can be given `enableForwarding` flag when [created](#creating-instance).

`enableForwarding` is `false` unless specified explicitly that seems never happen.

When enabled, `KafkaServer` creates a [ForwardingManager](#forwardingManager) and uses [BrokerToControllerChannelManager](#clientToControllerChannelManager).

## <span id="dynamicConfigManager"> ZkConfigManager

`KafkaServer` creates a [ZkConfigManager](../dynamic-broker-configuration/ZkConfigManager.md) when [started](#startup) with the following:

* [KafkaZkClient](#zkClient)
* [ConfigHandlers](#dynamicConfigHandlers)

`KafkaServer` requests the `ZkConfigManager` to [startup](../dynamic-broker-configuration/ZkConfigManager.md#startup) immediately.

### <span id="dynamicConfigHandlers"> ConfigHandlers

```scala
dynamicConfigHandlers: Map[String, ConfigHandler]
```

`KafkaServer` uses `dynamicConfigHandlers` registry of [ConfigHandler](../dynamic-broker-configuration/ConfigHandler.md)s (by their name).

Name | ConfigHandler
-----|--------------
 topics | [TopicConfigHandler](../dynamic-broker-configuration/TopicConfigHandler.md)
 clients | `ClientIdConfigHandler`
 users | `UserConfigHandler`
 brokers | [BrokerConfigHandler](../dynamic-broker-configuration/BrokerConfigHandler.md)
 ips | `IpConfigHandler`

`KafkaServer` uses the `dynamicConfigHandlers` to create [ZkConfigManager](#dynamicConfigManager) (at [startup](#startup)).

## <span id="transactionCoordinator"> TransactionCoordinator

`KafkaServer` creates and starts a [TransactionCoordinator](../transactions/TransactionCoordinator.md) when [created](#creating-instance).

`KafkaServer` uses the `TransactionCoordinator` to create the following:

* [data-plane](#dataPlaneRequestProcessor) and the [control-plane](#controlPlaneRequestProcessor) request processors
* [AutoTopicCreationManager](../AutoTopicCreationManager.md)

The `TransactionCoordinator` is requested to [shutdown](../transactions/TransactionCoordinator.md#shutdown) along with [KafkaServer](#shutdown).

## <span id="dataPlaneRequestProcessor"> Data-Plane Request Processor

`KafkaServer` creates a [KafkaApis](../KafkaApis.md) for data-related communication.

`KafkaApis` is used to create [data-plane request handler pool](#dataPlaneRequestHandlerPool).

## <span id="dataPlaneRequestHandlerPool"> KafkaRequestHandlerPool

## <span id="controlPlaneRequestProcessor"> Control-Plane Request Processor

`KafkaServer` creates a [KafkaApis](../KafkaApis.md) for control-related communication.

## <span id="startup"> Starting Up

```scala
startup(): Unit
```

`startup` is part of the [Server](../Server.md#startup) abstraction.

---

`startup` prints out the following INFO message to the logs:

```text
starting
```

`startup` [initZkClient](#initZkClient) and creates a [ZkConfigRepository](#configRepository) (with a new `AdminZkClient`).

`startup`...FIXME

`startup` [getOrGenerateClusterId](#getOrGenerateClusterId) (that becomes the [_clusterId](#_clusterId)) and prints out the following INFO message to the logs:

```text
Cluster ID = [clusterId]
```

`startup` [getBrokerMetadataAndOfflineDirs](../BrokerMetadataCheckpoint.md#getBrokerMetadataAndOfflineDirs) with the [logDirs](../KafkaConfig.md#logDirs).

`startup` [looks up the broker ID](#getOrGenerateBrokerId).

`startup`...FIXME

`startup` creates a [LogManager](#_logManager) that is requested to [start up](../log/LogManager.md#startup).

`startup`...FIXME

`startup` creates a [TransactionCoordinator](#transactionCoordinator) (with the [ReplicaManager](#replicaManager)) and requests it to [startup](../transactions/TransactionCoordinator.md#startup).

`startup`...FIXME

## <span id="KafkaBroker"> KafkaBroker

`KafkaServer` is a [KafkaBroker](KafkaBroker.md).

## <span id="getOrGenerateBrokerId"> Looking Up Broker ID

```scala
getOrGenerateBrokerId(
  brokerMetadata: RawMetaProperties): Int
```

`getOrGenerateBrokerId` takes the [broker.id](../KafkaConfig.md#brokerId) (from the [KafkaConfig](#config)) and makes sure that it matches the `RawMetaProperties`'s (or an `InconsistentBrokerIdException` is thrown).

`getOrGenerateBrokerId` uses the given `RawMetaProperties` for the broker ID if defined.

Otherwise, if `broker.id` (from the [KafkaConfig](../KafkaConfig.md#brokerId)) is negative and [broker.id.generation.enable](../KafkaConfig.md#brokerIdGenerationEnable) is enabled, `getOrGenerateBrokerId` [generates a broker ID](#generateBrokerId).

In the end, when all the earlier attempts "fail", `getOrGenerateBrokerId` uses the [broker.id](../KafkaConfig.md#brokerId) (from the [KafkaConfig](../KafkaConfig.md#brokerId)).

---

`getOrGenerateBrokerId` is used when:

* `KafkaServer` is requested to [start up](#startup)

## <span id="logManager"><span id="_logManager"> LogManager

```scala
logManager: LogManager
```

`logManager` is part of the [KafkaBroker](KafkaBroker.md#logManager) abstraction.

---

`KafkaServer` creates a [LogManager](../log/LogManager.md) at [startup](#startup).

## <span id="authorizer"> Authorizer

```scala
authorizer: Option[Authorizer]
```

`authorizer` is part of the [KafkaBroker](KafkaBroker.md#authorizer) abstraction.

---

`KafkaServer` is given an [Authorizer](../authorization/Authorizer.md) at [startup](#startup) based on [authorizer.class.name](../KafkaConfig.md#authorizer) configuration property.

## <span id="KafkaController"><span id="_kafkaController"><span id="kafkaController"> KafkaController

`KafkaServer` creates a [KafkaController](../controller/KafkaController.md) at [startup](#startup).

The `KafkaController` is requested to [start up](../controller/KafkaController.md#startup) immediately and [shut down](../controller/KafkaController.md#shutdown) alongside the [KafkaServer](#shutdown).

The `KafkaController` is used when:

* Creating a `TopicConfigHandler` (in the [dynamicConfigHandlers](#dynamicConfigHandlers))
* [controlledShutdown](#controlledShutdown) (for the [brokerEpoch](../controller/KafkaController.md#brokerEpoch))
* `DynamicLogConfig` is requested to `reconfigure`
* `DynamicListenerConfig` is requested to `reconfigure`
* `KafkaServer` is requested to [startup](#startup)
    * for the [brokerEpoch](../controller/KafkaController.md#brokerEpoch) for [AlterIsrManager](#alterIsrManager)
    * for the [brokerEpoch](../controller/KafkaController.md#brokerEpoch) for [ProducerIdManager](#alterIsrManager)
    * `ZkSupport`

## Logging

Enable `ALL` logging level for `kafka.server.KafkaServer` logger to see what happens inside.

Add the following line to `log4j.properties`:

```text
log4j.logger.kafka.server.KafkaServer=ALL
```

Refer to [Logging](../logging.md).
