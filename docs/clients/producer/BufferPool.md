# BufferPool

`BufferPool` is [created](#creating-instance) alongsize [KafkaProducer](KafkaProducer.md) for the [RecordAccumulator](KafkaProducer.md#accumulator).

## Creating Instance

`BufferPool` takes the following to be created:

* [Total Memory Size](#memory)
* [Batch Size](#poolableSize)
* <span id="metrics"> [Metrics](../../metrics/Metrics.md)
* <span id="time"> `Time`
* <span id="metricGrpName"> Metric Group Name

`BufferPool` is created when:

* `KafkaProducer` is [created](KafkaProducer.md) (to create a [RecordAccumulator](KafkaProducer.md#accumulator))

### Total Memory Size { #memory }

`BufferPool` is given the total memory size when [created](#creating-instance).

The size is by default the value of [buffer.memory](ProducerConfig.md#buffer.memory) configuration property.

### Batch Size { #poolableSize }

`BufferPool` is given the batch size when [created](#creating-instance).

The size is by default the value of [batch.size](ProducerConfig.md#batch.size) configuration property.

## Metrics

`BufferPool` registers the metrics under the [producer-metrics](KafkaProducer.md#PRODUCER_METRIC_GROUP_NAME) group name.

### buffer-exhausted-rate { #buffer-exhausted-rate }

The average per-second number of record sends that are dropped due to buffer exhaustion

### buffer-exhausted-records { #buffer-exhausted-records }

### buffer-exhausted-total { #buffer-exhausted-total }

The total number of record sends that are dropped due to buffer exhaustion

### bufferpool-wait-ratio { #bufferpool-wait-ratio }

The fraction of time an appender waits for space allocation

### bufferpool-wait-time-ns-total { #bufferpool-wait-time-ns-total }

The total time (in ns) an appender waits for space allocation
