# Server

`Server` is an [abstraction](#contract) of [Kafka servers](#implementations) (_brokers_).

## Contract

### <span id="awaitShutdown"> awaitShutdown

```scala
awaitShutdown(): Unit
```

Used when:

* FIXME

### <span id="startup"> startup

```scala
startup(): Unit
```

Used when:

* `Kafka` utility is [executed](Kafka.md#main) (on command line)

### <span id="shutdown"> shutdown

```scala
shutdown(): Unit
```

Used when:

* FIXME

## Implementations

* [KafkaRaftServer](KafkaRaftServer.md)
* [KafkaServer](KafkaServer.md)
