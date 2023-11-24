# Server

`Server` is an [abstraction](#contract) of [Kafka servers](#implementations) (_brokers_) for [Kafka](Kafka.md) utility.

## Contract

### <span id="awaitShutdown"> awaitShutdown

```scala
awaitShutdown(): Unit
```

Awaits termination signal (and keeps this server alive)

Used when:

* `Kafka` utility is [executed](Kafka.md#main) (on command line)

### <span id="startup"> Starting Up

```scala
startup(): Unit
```

Starts up this server

Used when:

* `Kafka` utility is [executed](Kafka.md#main) (on command line)

### <span id="shutdown"> shutdown

```scala
shutdown(): Unit
```

Shuts down this server (as part of `kafka-shutdown-hook` shutdown hook)

Used when:

* `Kafka` utility is [executed](Kafka.md#main) (on command line and handles termination signals)

## Implementations

* [KafkaRaftServer](kraft/KafkaRaftServer.md)
* [KafkaServer](broker/KafkaServer.md)
