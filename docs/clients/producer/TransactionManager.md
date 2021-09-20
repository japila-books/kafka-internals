# TransactionManager

## Creating Instance

`TransactionManager` takes the following to be created:

* <span id="logContext"> `LogContext`
* <span id="transactionalId"> [transactional.id](ProducerConfig.md#TRANSACTIONAL_ID_CONFIG)
* <span id="transactionTimeoutMs"> [transaction.timeout.ms](ProducerConfig.md#TRANSACTION_TIMEOUT_CONFIG)
* <span id="retryBackoffMs"> [retry.backoff.ms](ProducerConfig.md#RETRY_BACKOFF_MS_CONFIG)
* <span id="apiVersions"> `ApiVersions`
* <span id="autoDowngradeTxnCommit"> [internal.auto.downgrade.txn.commit](ProducerConfig.md#AUTO_DOWNGRADE_TXN_COMMIT)

`TransactionManager` is created together with [KafkaProducer](KafkaProducer.md#transactionManager) (with [idempotenceEnabled](ProducerConfig.md#idempotenceEnabled)).

## <span id="beginAbort"> beginAbort

```java
TransactionalRequestResult beginAbort()
```

`beginAbort`...FIXME

`beginAbort` is used when:

* `KafkaProducer` is requested to [abortTransaction](KafkaProducer.md#abortTransaction)
* `Sender` is requested to [run](Sender.md#run) (and is shuting down)

## <span id="beginCommit"> beginCommit

```java
TransactionalRequestResult beginCommit()
```

`beginCommit`...FIXME

`beginCommit` is used when:

* `KafkaProducer` is requested to [commitTransaction](KafkaProducer.md#commitTransaction)

## <span id="beginCompletingTransaction"> beginCompletingTransaction

```java
TransactionalRequestResult beginCompletingTransaction(
  TransactionResult transactionResult)
```

`beginCompletingTransaction`...FIXME

`beginCompletingTransaction` is used when:

* `TransactionManager` is requested to [beginCommit](#beginCommit) and [beginAbort](#beginAbort)

## <span id="beginTransaction"> beginTransaction

```java
void beginTransaction()
```

`beginTransaction`...FIXME

`beginTransaction` is used when:

* `KafkaProducer` is requested to [beginTransaction](KafkaProducer.md#beginTransaction)

## <span id="initializeTransactions"> initializeTransactions

```java
TransactionalRequestResult initializeTransactions() // (1)
TransactionalRequestResult initializeTransactions(
  ProducerIdAndEpoch producerIdAndEpoch)
```

1. Uses `ProducerIdAndEpoch.NONE`

`initializeTransactions`...FIXME

`initializeTransactions` is used when:

* `KafkaProducer` is requested to [initTransactions](KafkaProducer.md#initTransactions)
* `TransactionManager` is requested to [beginCompletingTransaction](#beginCompletingTransaction)

## <span id="isTransactional"> isTransactional

```java
boolean isTransactional()
```

`isTransactional` is `true` when the [transactionalId](#transactionalId) is defined.

## <span id="maybeAddPartitionToTransaction"> maybeAddPartitionToTransaction

```java
void maybeAddPartitionToTransaction(
  TopicPartition topicPartition)
```

`maybeAddPartitionToTransaction`...FIXME

`maybeAddPartitionToTransaction` is used when:

* `KafkaProducer` is requested to [doSend](KafkaProducer.md#doSend)

## <span id="sendOffsetsToTransaction"> sendOffsetsToTransaction

```java
TransactionalRequestResult sendOffsetsToTransaction(
  Map<TopicPartition, OffsetAndMetadata> offsets,
  ConsumerGroupMetadata groupMetadata)
```

`sendOffsetsToTransaction`...FIXME

`sendOffsetsToTransaction` is used when:

* `KafkaProducer` is requested to [sendOffsetsToTransaction](KafkaProducer.md#sendOffsetsToTransaction)
