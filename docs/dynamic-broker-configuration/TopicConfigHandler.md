# TopicConfigHandler

`TopicConfigHandler` is a [ConfigHandler](ConfigHandler.md).

## Creating Instance

`TopicConfigHandler` takes the following to be created:

* <span id="logManager"> [LogManager](../log/LogManager.md)
* <span id="kafkaConfig"> [KafkaConfig](../KafkaConfig.md)
* <span id="quotas"> `QuotaManagers`
* <span id="kafkaController"> Optional [KafkaController](../controller/KafkaController.md)

`TopicConfigHandler` is created when:

* `BrokerServer` is requested to [startup](../raft/BrokerServer.md#startup) (and create [dynamicConfigHandlers](../raft/BrokerServer.md#dynamicConfigHandlers))
* `KafkaServer` is requested to [startup](../broker/KafkaServer.md#startup) (and create [dynamicConfigHandlers](../broker/KafkaServer.md#dynamicConfigHandlers))

## <span id="processConfigChanges"> processConfigChanges

```scala
processConfigChanges(
  topic: String,
  topicConfig: Properties): Unit
```

`processConfigChanges` is part of the [ConfigHandler](ConfigHandler.md#processConfigChanges) abstraction.

---

`processConfigChanges`...FIXME

## Logging

Enable `ALL` logging level for `kafka.server.TopicConfigHandler` logger to see what happens inside.

Add the following line to `config/log4j.properties`:

```text
log4j.logger.kafka.server.TopicConfigHandler=ALL
```

Refer to [Logging](../logging.md).
