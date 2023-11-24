# KafkaBroker

`KafkaBroker` is an [abstraction](#contract) of [Kafka brokers](#implementations).

Every `KafkaBroker` is a [KafkaMetricsGroup](../metrics/KafkaMetricsGroup.md).

## Contract

### <span id="logManager"> LogManager

```scala
logManager: LogManager
```

[LogManager](../log/LogManager.md)

Used when:

* `DynamicBrokerConfig` is requested to `addReconfigurables`
* `DynamicThreadPool` is requested to `reconfigure`

### <span id="startup"> Starting Up

```scala
startup(): Unit
```

Used when:

* `KafkaServerTestHarness` is requested to `restartDeadBrokers` and `createBrokers`

### <span id="others"> others

!!! note
    There are other methods.

## Implementations

* [BrokerServer](../kraft/BrokerServer.md) (for [KRaft mode](../kraft/index.md))
* [KafkaServer](KafkaServer.md)
