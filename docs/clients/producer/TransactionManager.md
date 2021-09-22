# TransactionManager

## Creating Instance

`TransactionManager` takes the following to be created:

* <span id="logContext"> `LogContext`
* <span id="transactionalId"> [transactional.id](ProducerConfig.md#TRANSACTIONAL_ID_CONFIG)
* <span id="transactionTimeoutMs"> [transaction.timeout.ms](ProducerConfig.md#TRANSACTION_TIMEOUT_CONFIG)
* <span id="retryBackoffMs"> [retry.backoff.ms](ProducerConfig.md#RETRY_BACKOFF_MS_CONFIG)
* <span id="apiVersions"> `ApiVersions`
* <span id="autoDowngradeTxnCommit"> [internal.auto.downgrade.txn.commit](ProducerConfig.md#AUTO_DOWNGRADE_TXN_COMMIT)

`TransactionManager` is created along with [KafkaProducer](KafkaProducer.md#transactionManager) (with [idempotenceEnabled](ProducerConfig.md#idempotenceEnabled)).

## <span id="State"><span id="transitionTo"><span id="states"> States

`TransactionManager` can be in one of the following states:

* UNINITIALIZED
* INITIALIZING
* READY
* IN_TRANSACTION
* COMMITTING_TRANSACTION
* ABORTING_TRANSACTION
* ABORTABLE_ERROR
* FATAL_ERROR

### <span id="isTransitionValid"> Valid Transitions

Source (Current) State | Target States | transitionTo
-----------------------|---------------|-------------
 ABORTABLE_ERROR | <ul><li>ABORTING_TRANSACTION</li><li>ABORTABLE_ERROR</li></ul> |
 ABORTING_TRANSACTION | <ul><li>INITIALIZING</li><li>READY</li></ul> | [beginAbort](#beginAbort)
 COMMITTING_TRANSACTION | <ul><li>READY</li><li>ABORTABLE_ERROR</li></ul> | [beginCommit](#beginCommit)
 IN_TRANSACTION | <ul><li>COMMITTING_TRANSACTION</li><li>ABORTING_TRANSACTION</li><li>ABORTABLE_ERROR</li></ul> | [beginTransaction](#beginTransaction)
 INITIALIZING | READY | <ul><li>[initializeTransactions](#initializeTransactions)</li><li> [bumpIdempotentEpochAndResetIdIfNeeded](#bumpIdempotentEpochAndResetIdIfNeeded)</li><li>[completeTransaction](#completeTransaction)</li></ul>
 READY | <ul><li>UNINITIALIZED</li><li>IN_TRANSACTION</li></ul> |  <ul><li>[completeTransaction](#completeTransaction)</li><li>`InitProducerIdHandler`</li></ul>
 UNINITIALIZED | INITIALIZING | [resetIdempotentProducerId](#resetIdempotentProducerId)
 _any state_ | FATAL_ERROR |

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

`beginTransaction` makes sure that the producer is [transactional](#ensureTransactional) and [transition to](#transitionTo) `IN_TRANSACTION` state.

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

`isTransactional` is enabled (`true`) when the [transactional.id](ProducerConfig.md#TRANSACTIONAL_ID_CONFIG) configuration property is defined (for the producer and the [transactionalId](#transactionalId) was given when [created](#creating-instance)).

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
