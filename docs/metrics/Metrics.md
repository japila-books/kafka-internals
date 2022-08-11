# Metrics

`Metrics` is a registry of sensors and performance metrics (of Kafka brokers and clients).

## Creating Instance

`Metrics` takes the following to be created:

* <span id="defaultConfig"> [MetricConfig](MetricConfig.md)
* <span id="reporters"> `MetricsReporter`s
* <span id="time"> `Time`
* <span id="enableExpiration"> `enableExpiration` flag
* <span id="metricsContext"> `MetricsContext`

`Metrics` is created when:

* `Server` utility is used to [buildMetrics](../Server.md#buildMetrics)
* `KafkaAdminClient` utility is used to `createInternal`
* `KafkaConsumer` utility is used to [buildMetrics](../clients/consumer/KafkaConsumer.md#buildMetrics)
* `KafkaProducer` is [created](../clients/producer/KafkaProducer.md#metrics)
* `KafkaStreams` ([Kafka Streams]({{ book.kafka_streams }}/KafkaStreams#getMetrics)) utility is used to `getMetrics`
* Kafka Connect clients

## <span id="addReporter"> addReporter

```java
void addReporter(
  MetricsReporter reporter)
```

`addReporter`...FIXME

`addReporter` is used when:

* `DynamicMetricsReporters` is requested to `createReporters`

## <span id="sensors"> sensors

`Metrics` defines `sensors` collection of metric `Sensor`s by name (`ConcurrentMap<String, Sensor>`).

`sensors` is empty when `Metrics` is [created](#creating-instance).

A new `Sensor` is added in [sensor](#sensor).

### <span id="sensor"> sensor

```java
Sensor sensor(
  String name,
  MetricConfig config,
  long inactiveSensorExpirationTimeSeconds,
  Sensor.RecordingLevel recordingLevel,
  Sensor... parents)
Sensor sensor(...) // (1)
```

1. There are others

`sensor` [looks up the sensor (by name)](#getSensor) and returns it immediately if available.

Otherwise, `sensor` creates a new `Sensor` and adds it to the [sensors](#sensors) registry.

In the end, `sensor` prints out the following TRACE message to the logs:

```text
Added sensor with name [name]
```

### <span id="getSensor"> getSensor

```java
Sensor getSensor(
  String name)
```

`getSensor` looks up the given `name` in the [sensors](#sensors) registry.

## Logging

Enable `ALL` logging level for `org.apache.kafka.common.metrics.Metrics` logger to see what happens inside.

Add the following line to `config/log4j.properties`:

```text
log4j.logger.org.apache.kafka.common.metrics.Metrics=ALL
```

Refer to [Logging](../logging.md).
