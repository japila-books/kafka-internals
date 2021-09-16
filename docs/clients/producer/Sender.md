# Sender

`Sender` is a `Runnable` ([Java]({{ java.api }}/java/lang/Runnable.html)) (and is intended to be executed by a thread).

## Creating Instance

`Sender` takes the following to be created:

* <span id="logContext"> `LogContext`
* [KafkaClient](#client)
* <span id="metadata"> `ProducerMetadata`
* <span id="accumulator"> [RecordAccumulator](RecordAccumulator.md)
* <span id="guaranteeMessageOrder"> `guaranteeMessageOrder` flag
* <span id="maxRequestSize"> maxRequestSize
* <span id="acks"> acks
* <span id="retries"> retries
* <span id="metricsRegistry"> `SenderMetricsRegistry`
* <span id="time"> `Time`
* <span id="requestTimeoutMs"> requestTimeoutMs
* <span id="retryBackoffMs"> retryBackoffMs
* <span id="transactionManager"> [TransactionManager](TransactionManager.md)
* <span id="apiVersions"> `ApiVersions`

`Sender` is created along with a [KafkaProducer](KafkaProducer.md#sender).

## <span id="client"> KafkaClient

`Sender` is given a [KafkaClient](../KafkaClient.md) when [created](#creating-instance).

## <span id="wakeup"> Waking Up

```java
void wakeup()
```

`wakeup` requests the [KafkaClient](#client) to [wakeup](../KafkaClient.md#wakeup).

`wakeup` is used when:

* `KafkaProducer` is requested to [initTransactions](KafkaProducer.md#initTransactions), [sendOffsetsToTransaction](KafkaProducer.md#sendOffsetsToTransaction), [commitTransaction](KafkaProducer.md#commitTransaction), [abortTransaction](KafkaProducer.md#abortTransaction), [doSend](KafkaProducer.md#doSend), [waitOnMetadata](KafkaProducer.md#waitOnMetadata), [flush](KafkaProducer.md#flush)
* `Sender` is requested to [initiateClose](#initiateClose)