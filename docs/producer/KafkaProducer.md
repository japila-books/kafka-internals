# KafkaProducer

`KafkaProducer<K, V>` is a concrete [Producer](Producer.md).

## Creating Instance

`KafkaProducer` takes the following to be created:

* <span id="config"><span id="producerConfig"> [ProducerConfig](ProducerConfig.md)
* <span id="keySerializer"> Key `Serializer<K>`
* <span id="valueSerializer"> Value `Serializer<V>`
* <span id="metadata"> ProducerMetadata
* <span id="kafkaClient"> KafkaClient
* <span id="interceptors"> `ProducerInterceptor<K, V>`s
* <span id="time"> Time

### <span id="configureTransactionState"> configureTransactionState

```java
TransactionManager configureTransactionState(
  ProducerConfig config,
  LogContext logContext)
```

`configureTransactionState`...FIXME

### <span id="newSender"> newSender

```java
Sender newSender(
  LogContext logContext,
  KafkaClient kafkaClient,
  ProducerMetadata metadata)
```

`newSender`...FIXME

### <span id="configureInflightRequests"> configureInflightRequests

```java
int configureInflightRequests(
  ProducerConfig config)
```

`configureInflightRequests` gives the value of the [max.in.flight.requests.per.connection](ProducerConfig.md#max.in.flight.requests.per.connection) (in the given `ProducerConfig`).

`configureInflightRequests` throws a `ConfigException` when the [idempotence is enabled](ProducerConfig.md#idempotenceEnabled) and the value of the [max.in.flight.requests.per.connection](ProducerConfig.md#max.in.flight.requests.per.connection) is above 5:

```text
Must set max.in.flight.requests.per.connection to at most 5 to use the idempotent producer.
```

### <span id="configureAcks"> configureAcks

```java
short configureAcks(
  ProducerConfig config,
  Logger log)
```

`configureAcks`...FIXME

## <span id="accumulator"> RecordAccumulator

`KafkaProducer` creates a [RecordAccumulator](RecordAccumulator.md) when [created](#creating-instance).

This `RecordAccumulator` is used for the following:

* Create a [Sender](#newSender)
* [append](RecordAccumulator.md#append) when [doSend](#doSend)
* [beginFlush](RecordAccumulator.md#beginFlush) when [flush](#flush)

## <span id="abortTransaction"> Aborting Incomplete Transaction

```java
void abortTransaction()
```

`abortTransaction` prints out the following INFO message to the logs:

```text
Aborting incomplete transaction
```

`abortTransaction`...FIXME

`abortTransaction` is part of the [Producer](Producer.md#abortTransaction) abstraction.

## <span id="send"> Sending Record

```java
Future<RecordMetadata> send(
  ProducerRecord<K, V> record,
  Callback callback)
```

`send`...FIXME

`send` is part of the [Producer](Producer.md#send) abstraction.

### <span id="doSend"> doSend

```java
Future<RecordMetadata> doSend(
  ProducerRecord<K, V> record,
  Callback callback)
```

`doSend`...FIXME

### <span id="partition"> partition

```java
int partition(
  ProducerRecord<K, V> record,
  byte[] serializedKey,
  byte[] serializedValue,
  Cluster cluster)
```

`partition` is the `partition` (of the given `ProducerRecord`) if defined or requests the [Partitioner](#partitioner) for the [partition](Partitioner.md#partition).

## <span id="flush"> Flushing

```java
void flush()
```

`flush` requests the [RecordAccumulator](#accumulator) to [beginFlush](RecordAccumulator.md#beginFlush).

`flush` requests the [Sender](#sender) to [wakeup](Sender.md#wakeup).

`flush` requests the [RecordAccumulator](#accumulator) to [awaitFlushCompletion](RecordAccumulator.md#awaitFlushCompletion).

`flush` is part of the [Producer](Producer.md#flush) abstraction.

## Demo

```text
// Necessary imports
import org.apache.kafka.clients.producer.KafkaProducer
import org.apache.kafka.clients.producer.ProducerConfig
import org.apache.kafka.common.serialization.StringSerializer

// Creating a KafkaProducer
import java.util.Properties
val props = new Properties()
props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, classOf[StringSerializer].getName)
props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, classOf[StringSerializer].getName)
props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, ":9092")
val producer = new KafkaProducer[String, String](props)

// Creating a record to be sent
import org.apache.kafka.clients.producer.ProducerRecord
val r = new ProducerRecord[String, String]("0", "this is a message")

// Sending the record (with no Callback)
import java.util.concurrent.Future
import org.apache.kafka.clients.producer.RecordMetadata
val metadataF: Future[RecordMetadata] = producer.send(r)
```

## Logging

Enable `ALL` logging level for `org.apache.kafka.clients.producer.KafkaProducer` logger to see what happens inside.

Add the following line to `log4j.properties`:

```text
log4j.logger.org.apache.kafka.clients.producer.KafkaProducer=ALL
```

Refer to [Logging](../logging.md).
