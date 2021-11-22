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
