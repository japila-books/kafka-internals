# BrokerReconfigurable

`BrokerReconfigurable` is an [abstraction](#contract) of [dynamic reconfigurables](#implementations) that can be [reconfigured](#reconfigure) at runtime.

## Contract

### <span id="reconfigurableConfigs"> Reconfigurable Configs

```scala
reconfigurableConfigs: Set[String]
```

Used when:

* `DynamicBrokerConfig` is requested to [addBrokerReconfigurable](DynamicBrokerConfig.md#addBrokerReconfigurable) and [processReconfiguration](DynamicBrokerConfig.md#processReconfiguration)

### <span id="validateReconfiguration"> validateReconfiguration

```scala
validateReconfiguration(
  newConfig: KafkaConfig): Unit
```

Validates the updated [KafkaConfig](../KafkaConfig.md)

Used when:

* `DynamicBrokerConfig` is requested to [processReconfiguration](DynamicBrokerConfig.md#processReconfiguration)

### <span id="reconfigure"> Reconfiguring Broker

```scala
reconfigure(
  oldConfig: KafkaConfig,
  newConfig: KafkaConfig): Unit
```

Used when:

* `DynamicBrokerConfig` is requested to [updateCurrentConfig](DynamicBrokerConfig.md#updateCurrentConfig)

## Implementations

* `DynamicListenerConfig`
* [DynamicLogConfig](DynamicLogConfig.md)
* [DynamicThreadPool](DynamicThreadPool.md)
* [LogCleaner](../log/LogCleaner.md)
* `SocketServer`
