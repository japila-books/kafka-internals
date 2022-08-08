# KafkaBroker

`KafkaBroker` is an [extension](#contract) of the [KafkaMetricsGroup](../metrics/KafkaMetricsGroup.md) abstraction for [Kafka brokers](#implementations).

## Contract

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

* [BrokerServer](../raft/BrokerServer.md) (for [KRaft mode](../raft/index.md))
* [KafkaServer](KafkaServer.md)
