# BrokerReconfigurable

`BrokerReconfigurable` is an [abstraction](#contract) of [FIXME](#implementations) that can...FIXME

## Contract

### <span id="reconfigurableConfigs"> reconfigurableConfigs

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

Used when:

* `DynamicBrokerConfig` is requested to [processReconfiguration](DynamicBrokerConfig.md#processReconfiguration)

### <span id="reconfigure"> reconfigure

```scala
reconfigure(
  oldConfig: KafkaConfig,
  newConfig: KafkaConfig): Unit
```

Used when:

* `DynamicBrokerConfig` is requested to [updateCurrentConfig](DynamicBrokerConfig.md#updateCurrentConfig)

## Implementations

* `DynamicListenerConfig`
* `DynamicLogConfig`
* [DynamicThreadPool](DynamicThreadPool.md)
* [LogCleaner](../log/LogCleaner.md)
* `SocketServer`
