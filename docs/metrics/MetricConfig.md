# MetricConfig

## Creating Instance

`MetricConfig` takes no arguments to be created.

`MetricConfig` is created when:

* _many places_ (FIXME)

## <span id="recordingLevel"> RecordingLevel

`MetricConfig` uses `INFO` recording level by default (when [created](#creating-instance)) that can be changed using [recordLevel](#recordLevel).

### <span id="recordLevel"> recordLevel

```java
MetricConfig recordLevel(
  Sensor.RecordingLevel recordingLevel)
```

`recordLevel` sets the [recordingLevel](#recordingLevel) to the given `RecordingLevel`.

`recordLevel` is used when:

* `KafkaAdminClient` is requested to `createInternal`
* `KafkaConsumer` is requested to [buildMetrics](../clients/consumer/KafkaConsumer.md#buildMetrics)
* `KafkaProducer` is [created](../clients/producer/KafkaProducer.md#metricConfig)
* `KafkaStreams` ([Kafka Streams]({{ book.kafka_streams }}/KafkaStreams#getMetrics)) is requested to `getMetrics`
* `StreamsMetricsImpl` ([Kafka Streams]({{ book.kafka_streams }}/metrics/StreamsMetricsImpl)) is requested to `addClientLevelImmutableMetric`, `addClientLevelMutableMetric`, `addStoreLevelMutableMetric`
* `Server` utility is used to [buildMetricsConfig](../Server.md#buildMetricsConfig)
