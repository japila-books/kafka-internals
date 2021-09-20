# KafkaProducer

`KafkaProducer<K, V>` is a concrete [Producer](Producer.md).

## Creating Instance

`KafkaProducer` takes the following to be created:

* <span id="config"><span id="producerConfig"> [ProducerConfig](ProducerConfig.md)
* <span id="keySerializer"> Key `Serializer<K>`
* <span id="valueSerializer"> Value `Serializer<V>`
* <span id="metadata"> ProducerMetadata
* <span id="kafkaClient"> [KafkaClient](../KafkaClient.md)
* <span id="interceptors"> `ProducerInterceptor<K, V>`s
* <span id="time"> Time

### <span id="configureTransactionState"> configureTransactionState

```java
TransactionManager configureTransactionState(
  ProducerConfig config,
  LogContext logContext)
```

`configureTransactionState` creates a new [TransactionManager](TransactionManager.md) or returns `null`.

---

`configureTransactionState` checks whether the following configuration properties are specified in the given [ProducerConfig](ProducerConfig.md):

1. [enable.idempotence](ProducerConfig.md#ENABLE_IDEMPOTENCE_CONFIG)
1. [transactional.id](ProducerConfig.md#TRANSACTIONAL_ID_CONFIG)

With [transactional.id](ProducerConfig.md#TRANSACTIONAL_ID_CONFIG) specified, `configureTransactionState` turns the [enable.idempotence](ProducerConfig.md#ENABLE_IDEMPOTENCE_CONFIG) on and prints out the following INFO message to the logs:

```text
Overriding the default [enable.idempotence] to true since transactional.id is specified.
```

With [idempotence enabled](ProducerConfig.md#idempotenceEnabled), `configureTransactionState` creates a [TransactionManager](TransactionManager.md) with the values of the following configuration properties:

1. [transactional.id](ProducerConfig.md#TRANSACTIONAL_ID_CONFIG)
1. [transaction.timeout.ms](ProducerConfig.md#TRANSACTION_TIMEOUT_CONFIG)
1. [retry.backoff.ms](ProducerConfig.md#RETRY_BACKOFF_MS_CONFIG)
1. [internal.auto.downgrade.txn.commit](ProducerConfig.md#AUTO_DOWNGRADE_TXN_COMMIT)

When the `TransactionManager` is [transactional](TransactionManager.md#isTransactional), `configureTransactionState` prints out the following INFO message to the logs:

```text
Instantiated a transactional producer.
```

Otherwise, `configureTransactionState` prints out the following INFO message to the logs:

```text
Instantiated an idempotent producer.
```

In the end, `configureTransactionState` returns the `TransactionManager` or `null`.

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

`configureAcks` returns the value of [acks](ProducerConfig.md#ACKS_CONFIG) configuration property (in the given [ProducerConfig](ProducerConfig.md)).

With [idempotenceEnabled](ProducerConfig.md#idempotenceEnabled), `configureAcks` prints out the following INFO message to the logs when there is no [acks](ProducerConfig.md#ACKS_CONFIG) configuration property defined:

```text
Overriding the default [acks] to all since idempotence is enabled.
```

With [idempotenceEnabled](ProducerConfig.md#idempotenceEnabled) and the `acks` not `-1`, `configureAcks` throws a `ConfigException`:

```text
Must set acks to all in order to use the idempotent producer.
Otherwise we cannot guarantee idempotence.
```

## <span id="transactionManager"> TransactionManager

`KafkaProducer` may create a [TransactionManager](TransactionManager.md) when [created](#configureTransactionState).

`TransactionManager` is used to create the following:

* [RecordAccumulator](RecordAccumulator.md#transactionManager)
* [Sender](Sender.md#transactionManager)

`KafkaProducer` uses the `TransactionManager` for the following transactional methods:

* [abortTransaction](#abortTransaction)
* [beginTransaction](#beginTransaction)
* [commitTransaction](#commitTransaction)
* [initTransactions](#initTransactions)
* [sendOffsetsToTransaction](#sendOffsetsToTransaction)
* [doSend](#doSend)

### <span id="throwIfNoTransactionManager"> throwIfNoTransactionManager

`KafkaProducer` throws an `IllegalStateException` for the transactional methods but [TransactionManager](#transactionManager) is not configured.

```text
Cannot use transactional methods without enabling transactions by setting the transactional.id configuration property
```

## <span id="sender"><span id="ioThread"> Sender Thread

`KafkaProducer` creates a [Sender](Sender.md) when [created](#newSender).

`Sender` is immediately started as a daemon thread with the following name (using the [clientId](#clientId)):

```text
kafka-producer-network-thread | [clientId]
```

`KafkaProducer` is actually considered open (and usable) as long as the `Sender` is [running](Sender.md#isRunning).

`KafkaProducer` simply requests the `Sender` to [wake up](Sender.md#wakeup) for the following:

* [initTransactions](#initTransactions)
* [sendOffsetsToTransaction](#sendOffsetsToTransaction)
* [commitTransaction](#commitTransaction)
* [abortTransaction](#abortTransaction)
* [doSend](#doSend)
* [waitOnMetadata](#waitOnMetadata)
* [flush](#flush)

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

## <span id="waitOnMetadata"> waitOnMetadata

```java
ClusterAndWaitTime waitOnMetadata(
  String topic,
  Integer partition,
  long nowMs,
  long maxWaitMs)
```

`waitOnMetadata` requests the [ProducerMetadata](#metadata) for the [current cluster info](../Metadata.md#fetch).

`waitOnMetadata`...FIXME

`waitOnMetadata` is used when:

* `KafkaProducer` is requested to [doSend](#doSend) and [partitionsFor](#partitionsFor)

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

Refer to [Logging](../../logging.md).
