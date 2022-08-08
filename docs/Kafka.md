# Kafka Utility

`kafka.Kafka` is a [command-line application](#main) that is executed using `kafka-server-start` shell script.

## <span id="main"> Launching Kafka Server

```scala
main(
  args: Array[String]): Unit
```

`main`...FIXME

### <span id="buildServer"> buildServer

```scala
buildServer(
  props: Properties): Server
```

`buildServer` [creates a KafkaConfig](KafkaConfig.md#fromProps) (from the given `Properties`).

`buildServer` creates a [Server](Server.md) based on [process.roles](KafkaConfig.md#requiresZookeeper) configuration property:

* [KafkaServer](broker/KafkaServer.md) if [empty](KafkaConfig.md#requiresZookeeper)
* [KafkaRaftServer](raft/KafkaRaftServer.md), otherwise
