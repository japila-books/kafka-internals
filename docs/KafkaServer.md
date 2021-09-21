# KafkaServer

`KafkaServer` is a [Server](Server.md) that [Kafka](Kafka.md) command-line application uses in Zookeeper mode (when executed with the [process.roles](KafkaConfig.md#process.roles) configuration property undefined).

## Creating Instance

`KafkaServer` takes the following to be created:

* <span id="config"> [KafkaConfig](KafkaConfig.md)
* <span id="time"> `Time` (default: `SYSTEM`)
* <span id="threadNamePrefix"> Optional Thread Name Prefix (default: undefined)
* <span id="enableForwarding"> `enableForwarding` flag (default: `false`)

`KafkaServer` is created when:

* `Kafka` command-line application is [launched](Kafka.md#main) (and [builds a server](Kafka.md#buildServer))

## <span id="transactionCoordinator"> TransactionCoordinator

`KafkaServer` creates and starts a [TransactionCoordinator](TransactionCoordinator.md) when [created](#creating-instance).

`KafkaServer` uses the `TransactionCoordinator` to create the following:

* [data-plane](#dataPlaneRequestProcessor) and the [control-plane](#controlPlaneRequestProcessor) request processors
* [AutoTopicCreationManager](AutoTopicCreationManager.md)

The `TransactionCoordinator` is requested to [shutdown](TransactionCoordinator.md#shutdown) together with [KafkaServer](#shutdown).

## <span id="dataPlaneRequestProcessor"> Data-Plane Request Processor

`KafkaServer` creates a [KafkaApis](KafkaApis.md) for data-related communication.

`KafkaApis` is used to create [data-plane request handler pool](#dataPlaneRequestHandlerPool).

## <span id="dataPlaneRequestHandlerPool"> KafkaRequestHandlerPool

## <span id="controlPlaneRequestProcessor"> Control-Plane Request Processor

`KafkaServer` creates a [KafkaApis](KafkaApis.md) for control-related communication.

## <span id="startup"> startup

```scala
startup(): Unit
```

`startup` prints out the following INFO message to the logs:

```text
starting
```

`startup` [initZkClient](#initZkClient) and creates a [ZkConfigRepository](#configRepository).

`startup`...FIXME

`startup` prints out the following INFO message to the logs:

```text
Cluster ID = [clusterId]
```

`startup`...FIXME

`startup` creates a [TransactionCoordinator](#transactionCoordinator) (with the [ReplicaManager](#replicaManager)) and requests it to [startup](TransactionCoordinator.md#startup).

`startup`...FIXME

`startup` is part of the [Server](Server.md#startup) abstraction.

## Logging

Enable `ALL` logging level for `kafka.server.KafkaServer` logger to see what happens inside.

Add the following line to `log4j.properties`:

```text
log4j.logger.kafka.server.KafkaServer=ALL
```

Refer to [Logging](logging.md).
