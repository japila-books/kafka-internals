# Sender

`Sender` is a `Runnable` ([Java]({{ java.api }}/java/lang/Runnable.html)) that is executed as a separate thread alongside [KafkaProducer](KafkaProducer.md#sender) to send records to a Kafka cluster.

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

`Sender` is created along with [KafkaProducer](KafkaProducer.md#sender).

## <span id="client"> KafkaClient

`Sender` is given a [KafkaClient](../KafkaClient.md) when [created](#creating-instance).

## <span id="run"> Running Thread

```java
void run()
```

`run` prints out the following DEBUG message to the logs:

```text
Starting Kafka producer I/O thread.
```

`run` [runs once](#runOnce) and repeats until the [running](#running) flag is turned off.

Right after the [running](#running) flag is off, `run` prints out the following DEBUG message to the logs:

```text
Beginning shutdown of Kafka producer I/O thread, sending remaining records.
```

`run`...FIXME

In the end, `run` prints out the following DEBUG message to the logs:

```text
Shutdown of Kafka producer I/O thread has completed.
```

`run` is part of the `Runnable` ([Java]({{ java.api }}/java/lang/Runnable.html#run())) abstraction.

### <span id="runOnce"> runOnce

```java
void runOnce()
```

If executed with a [TransactionManager](#transactionManager), `runOnce`...FIXME

`runOnce` [sendProducerData](#sendProducerData).

`runOnce` requests the [KafkaClient](#client) to [poll](../KafkaClient.md#poll).

### <span id="sendProducerData"> sendProducerData

```java
long sendProducerData(
  long now)
```

`sendProducerData` requests the [ProducerMetadata](#metadata) for the [current cluster info](../Metadata.md#fetch)

`sendProducerData` requests the [RecordAccumulator](#accumulator) for the [partitions with data ready to send](RecordAccumulator.md#ready).

`sendProducerData` requests a metadata update when there are partitions with no leaders.

`sendProducerData` removes nodes not ready to send to.

`sendProducerData` requests the [RecordAccumulator](#accumulator) to [drain](RecordAccumulator.md#drain) (and create [ProducerBatch](ProducerBatch.md)s).

`sendProducerData` [registers the batches](#addToInflightBatches) (in the [inFlightBatches](#inFlightBatches) registry).

With [guaranteeMessageOrder](#guaranteeMessageOrder), `sendProducerData` mutes all the partitions drained.

`sendProducerData` requests the [RecordAccumulator](#accumulator) to [resetNextBatchExpiryTime](RecordAccumulator.md#resetNextBatchExpiryTime).

`sendProducerData` requests the [RecordAccumulator](#accumulator) for the [expired batches](RecordAccumulator.md#expiredBatches) and adds all [expired InflightBatches](#getExpiredInflightBatches).

If there are any expired batches, `sendProducerData`...FIXME

`sendProducerData` requests the `SenderMetrics` to `updateProduceRequestMetrics`.

With at least one broker to send batches to, `sendProducerData` prints out the following TRACE message to the logs:

```text
Nodes with data ready to send: [readyNodes]
```

`sendProducerData` [sendProduceRequests](#sendProduceRequests).

### <span id="sendProduceRequests"> sendProduceRequests

```java
void sendProduceRequests(
  Map<Integer, List<ProducerBatch>> collated,
  long now)
```

For every pair of a broker node and an associated [ProducerBatch](ProducerBatch.md) (in the given `collated` collection), `sendProduceRequests` [sendProduceRequest](#sendProduceRequest) with the broker node, the [acks](#acks), the [requestTimeoutMs](#requestTimeoutMs) and the `ProducerBatch`.

### <span id="sendProduceRequest"> sendProduceRequest

```java
void sendProduceRequest(
  long now,
  int destination,
  short acks,
  int timeout,
  List<ProducerBatch> batches)
```

`sendProduceRequest` creates a collection of [ProducerBatch](ProducerBatch.md)es by `TopicPartition` from the given `batches`.

`sendProduceRequest` requests the [KafkaClient](#client) for a new [ClientRequest](../KafkaClient.md#newClientRequest) (for the `destination` broker) and to [send it](../KafkaClient.md#send).

`sendProduceRequest` registers a [handleProduceResponse] callback to invoke when a response arrives. `sendProduceRequest` expects a response for all the `acks` but `0`.

In the end, `sendProduceRequest` prints out the following TRACE message to the logs:

```text
Sent produce request to [nodeId]: [requestBuilder]
```

### <span id="handleProduceResponse"> handleProduceResponse

```java
void handleProduceResponse(
  ClientResponse response,
  Map<TopicPartition, ProducerBatch> batches,
  long now)
```

### <span id="maybeSendAndPollTransactionalRequest"> maybeSendAndPollTransactionalRequest

```java
boolean maybeSendAndPollTransactionalRequest()
```

## <span id="running"><span id="isRunning"> running Flag

`Sender` [runs](#run) as long as the `running` internal flag is on.

The `running` flag is on from when `Sender` is [created](#creating-instance) until requested to [initiateClose](#initiateClose).

## <span id="initiateClose"> initiateClose

```java
void initiateClose()
```

`initiateClose` requests the [RecordAccumulator](#accumulator) to [close](RecordAccumulator.md#close) and turns the [running](#running) flag off.

In the end, `initiateClose` [wakes up the KafkaClient](#wakeup).

`initiateClose` is used when:

* `KafkaProducer` is requested to [close](KafkaProducer.md#close)
* `Sender` is requested to [forceClose](#forceClose)

## <span id="wakeup"> Waking Up

```java
void wakeup()
```

`wakeup` requests the [KafkaClient](#client) to [wakeup](../KafkaClient.md#wakeup).

`wakeup` is used when:

* `KafkaProducer` is requested to [initTransactions](KafkaProducer.md#initTransactions), [sendOffsetsToTransaction](KafkaProducer.md#sendOffsetsToTransaction), [commitTransaction](KafkaProducer.md#commitTransaction), [abortTransaction](KafkaProducer.md#abortTransaction), [doSend](KafkaProducer.md#doSend), [waitOnMetadata](KafkaProducer.md#waitOnMetadata), [flush](KafkaProducer.md#flush)
* `Sender` is requested to [initiateClose](#initiateClose)

## Logging

Enable `ALL` logging level for `org.apache.kafka.clients.producer.internals.Sender` logger to see what happens inside.

Add the following line to `log4j.properties`:

```text
log4j.logger.org.apache.kafka.clients.producer.internals.Sender=ALL
```

Refer to [Logging](../../logging.md).
