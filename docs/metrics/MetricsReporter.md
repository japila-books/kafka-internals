# MetricsReporter

`MetricsReporter` is an [extension](#contract) of the [Reconfigurable](../dynamic-configuration/Reconfigurable.md) abstraction for [reconfigurable metrics reporters](#implementations).

## Contract (Subset)

### <span id="init"> init

```java
void init(
  List<KafkaMetric> metrics)
```

Used when:

* `Metrics` is [created](Metrics.md#creating-instance) and requested to [addReporter](Metrics.md#addReporter)

## Implementations

* `JmxReporter`
* `PushHttpMetricsReporter`
