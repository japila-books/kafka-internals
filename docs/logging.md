# Logging

## Brokers

Kafka brokers use [Apache Log4j 2](https://logging.apache.org/log4j/2.x/) for logging and use `config/log4j.properties` by default.

The default logging level is `INFO` with `stdout` appender.

```text
log4j.rootLogger=INFO, stdout, kafkaAppender

log4j.logger.kafka=INFO
log4j.logger.org.apache.kafka=INFO
```

## Clients

### build.sbt

```text
libraryDependencies += "org.apache.kafka" % "kafka-clients" % "2.8.0"

val slf4j = "1.7.32"
libraryDependencies += "org.slf4j" % "slf4j-api" % slf4j
libraryDependencies += "org.slf4j" % "slf4j-log4j12" % slf4j
```

### log4j.properties

In `src/main/resources/log4j.properties` use the following:

```text
log4j.rootLogger=INFO, stdout

log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=[%d] %p %m (%c)%n

log4j.logger.org.apache.kafka.clients.producer.KafkaProducer=ALL
```
