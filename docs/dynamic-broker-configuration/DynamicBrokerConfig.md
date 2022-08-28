# DynamicBrokerConfig

## <span id="initialize"> Initializing

```scala
initialize(
  zkClientOpt: Option[KafkaZkClient]): Unit
```

---

`initialize` creates a new [KafkaConfig](#currentConfig) (based on the given current [KafkaConfig](#kafkaConfig)).

With a `KafkaZkClient` specified ([KafkaServer](../broker/KafkaServer.md#startup)):

1. `initialize` creates a [AdminZkClient](../zk/AdminZkClient.md)
1. `initialize` uses the `AdminZkClient` to [fetchEntityConfig](../zk/AdminZkClient.md#fetchEntityConfig) for all `brokers` and then [updateDefaultConfig](#updateDefaultConfig)

    ```shell
    ./bin/zkCli.sh -server localhost:2181 get /config/brokers
    ```

1. `initialize` uses the `AdminZkClient` to [fetchEntityConfig](../zk/AdminZkClient.md#fetchEntityConfig) for this broker (`brokers` entity with [broker.id](../KafkaConfig.md#brokerId)) and then [updateBrokerConfig](#updateBrokerConfig)

    ```shell
    ./bin/zkCli.sh -server localhost:2181 get /config/brokers/[broker.id]
    ```

!!! note
    `KafkaZkClient` is specified for [KafkaServer](../broker/KafkaServer.md#startup) (unlike [BrokerServer](../raft/BrokerServer.md#startup) that makes sense since `BrokerServer` is for Zookeeper-less Kafka deployment).

---

`initialize` is used when:

* `BrokerServer` is requested to [start up](../raft/BrokerServer.md#startup)
* `KafkaServer` is requested to [start up](../broker/KafkaServer.md#startup)

## <span id="addReconfigurables"> addReconfigurables

```scala
addReconfigurables(
  kafkaServer: KafkaBroker): Unit
```

`addReconfigurables`...FIXME

---

`addReconfigurables` is used when:

* `BrokerServer` is requested to [start up](../raft/BrokerServer.md#startup)
* `KafkaServer` is requested to [start up](../broker/KafkaServer.md#startup)

## <span id="addBrokerReconfigurable"> addBrokerReconfigurable

```scala
addBrokerReconfigurable(
  reconfigurable: BrokerReconfigurable): Unit
```

`addBrokerReconfigurable`...FIXME

---

`addBrokerReconfigurable` is used when:

* FIXME

## <span id="updateDefaultConfig"> updateDefaultConfig

```scala
updateDefaultConfig(
  persistentProps: Properties,
  doLog: Boolean = true): Unit
```

`updateDefaultConfig`...FIXME

---

`updateDefaultConfig` is used when:

* `BrokerConfigHandler` is requested to [processConfigChanges](BrokerConfigHandler.md#processConfigChanges)
* `DynamicBrokerConfig` is requested to [initialize](#initialize)

## <span id="processReconfiguration"> processReconfiguration

```scala
processReconfiguration(
  newProps: Map[String, String],
  validateOnly: Boolean,
  doLog: Boolean = false): (KafkaConfig, List[BrokerReconfigurable])
```

`processReconfiguration`...FIXME

---

`processReconfiguration` is used when:

* `DynamicBrokerConfig` is requested to [validate](#validate) and [updateCurrentConfig](#updateCurrentConfig)

## <span id="validate"> validate

```scala
validate(
  props: Properties,
  perBrokerConfig: Boolean): Unit
```

`validate`...FIXME

---

`validate` is used when:

* FIXME

## <span id="updateCurrentConfig"> updateCurrentConfig

```scala
updateCurrentConfig(
  doLog: Boolean): Unit
```

`updateCurrentConfig`...FIXME

---

`updateCurrentConfig` is used when:

* `DynamicBrokerConfig` is requested to [updateBrokerConfig](#updateBrokerConfig) and [updateDefaultConfig](#updateDefaultConfig)

## <span id="updateBrokerConfig"> updateBrokerConfig

```scala
updateBrokerConfig(
  brokerId: Int,
  persistentProps: Properties,
  doLog: Boolean = true): Unit
```

`updateBrokerConfig`...FIXME

---

`updateBrokerConfig` is used when:

* `BrokerConfigHandler` is requested to [processConfigChanges](BrokerConfigHandler.md#processConfigChanges)
* `DynamicBrokerConfig` is requested to [initialize](#initialize)

## Logging

Enable `ALL` logging level for `kafka.server.DynamicBrokerConfig` logger to see what happens inside.

Add the following line to `config/log4j.properties`:

```text
log4j.logger.kafka.server.DynamicBrokerConfig=ALL
```

Refer to [Logging](../logging.md).
