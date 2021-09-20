# Kafka Utility

`Kafka` is a [command-line application](#main).

## <span id="main"> main

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

`buildServer` creates a [KafkaServer](KafkaServer.md) or [KafkaRaftServer](KafkaRaftServer.md) based on [process.roles](KafkaConfig.md#requiresZookeeper) configuration property (in the given `Properties`).
