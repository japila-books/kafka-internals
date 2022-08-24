# TransactionCoordinator

`TransactionCoordinator` runs on every Kafka broker ([BrokerServer](../raft/BrokerServer.md) or [KafkaServer](../broker/KafkaServer.md#)).

## Creating Instance

`TransactionCoordinator` takes the following to be created:

* <span id="brokerId"> Broker Id
* <span id="txnConfig"> [TransactionConfig](TransactionConfig.md)
* <span id="scheduler"> `Scheduler`
* <span id="createProducerIdGenerator"> `createProducerIdGenerator` function (`() => ProducerIdGenerator`)
* <span id="txnManager"> [TransactionStateManager](TransactionStateManager.md)
* <span id="txnMarkerChannelManager"> `TransactionMarkerChannelManager`
* <span id="time"> `Time`
* <span id="logContext"> `LogContext`

`TransactionCoordinator` is created using [apply](#apply) factory.

## <span id="apply"> Creating TransactionCoordinator

```scala
apply(
  config: KafkaConfig,
  replicaManager: ReplicaManager,
  scheduler: Scheduler,
  createProducerIdGenerator: () => ProducerIdGenerator,
  metrics: Metrics,
  metadataCache: MetadataCache,
  time: Time): TransactionCoordinator
```

`apply` creates a [TransactionConfig](TransactionConfig.md).

`apply` creates a [TransactionStateManager](TransactionStateManager.md) (with the [brokerId](../KafkaConfig.md#brokerId) and the other Kafka services).

`apply` creates a `LogContext` that uses the following log prefix (with the [brokerId](../KafkaConfig.md#brokerId)):

```text
[TransactionCoordinator id=[brokerId]]
```

`apply` creates a `TransactionMarkerChannelManager`.

In the end, `apply` creates a [TransactionCoordinator](#creating-instance).

`apply` is used when:

* `BrokerServer` is requested to [start up](../raft/BrokerServer.md#startup)
* `KafkaServer` is requested to [start up](../broker/KafkaServer.md#startup)

## <span id="startup"> Starting Up

```scala
startup(
  retrieveTransactionTopicPartitionCount: () => Int,
  enableTransactionalIdExpiration: Boolean = true): Unit
```

`startup`...FIXME

`startup` is used when:

* `BrokerServer` is requested to [start up](../raft/BrokerServer.md#startup)
* `KafkaServer` is requested to [start up](../broker/KafkaServer.md#startup)

## <span id="onElection"> onElection

```scala
onElection(
  txnTopicPartitionId: Int,
  coordinatorEpoch: Int): Unit
```

`onElection` prints out the following INFO message to the logs:

```text
Elected as the txn coordinator for partition [txnTopicPartitionId] at epoch [coordinatorEpoch]
```

`onElection` requests the [TransactionMarkerChannelManager](#txnMarkerChannelManager) to [removeMarkersForTxnTopicPartition](TransactionMarkerChannelManager.md#removeMarkersForTxnTopicPartition) for the given `txnTopicPartitionId` partition.

In the end, `onElection` requests the [TransactionStateManager](#txnManager) to [loadTransactionsForTxnTopicPartition](TransactionStateManager.md#loadTransactionsForTxnTopicPartition).

`onElection` is used when:

* `RequestHandlerHelper` is requested to [onLeadershipChange](../RequestHandlerHelper.md#onLeadershipChange)

## <span id="onResignation"> onResignation

```scala
onResignation(
  txnTopicPartitionId: Int,
  coordinatorEpoch: Option[Int]): Unit
```

`onResignation`...FIXME

`onResignation` is used when:

* `KafkaApis` is requested to [handleStopReplicaRequest](../KafkaApis.md#handleStopReplicaRequest)
* `RequestHandlerHelper` is requested to [onLeadershipChange](../RequestHandlerHelper.md#onLeadershipChange)

## <span id="handleInitProducerId"> handleInitProducerId

```scala
handleInitProducerId(
  transactionalId: String,
  transactionTimeoutMs: Int,
  expectedProducerIdAndEpoch: Option[ProducerIdAndEpoch],
  responseCallback: InitProducerIdCallback): Unit
```

For `transactionalId` undefined (`null`), `handleInitProducerId` requests the [ProducerIdGenerator](#createProducerIdGenerator) to `generateProducerId` and sends it back (using the given `InitProducerIdCallback`).

`handleInitProducerId` requests the [TransactionStateManager](#txnManager) to [getTransactionState](TransactionStateManager.md#getTransactionState) for the given `transactionalId`.

`handleInitProducerId` prints out the following INFO message to the logs:

```text
Initialized transactionalId [transactionalId] with producerId [producerId] and producer epoch [producerEpoch]
on partition __transaction_state-[partition]
```

In the end, `handleInitProducerId` requests the [TransactionStateManager](#txnManager) to [appendTransactionToLog](TransactionStateManager.md#appendTransactionToLog).

`handleInitProducerId` is used when:

* `KafkaApis` is requested to [handleInitProducerIdRequest](../KafkaApis.md#handleInitProducerIdRequest)

## <span id="transactionTopicConfigs"> transactionTopicConfigs

```scala
transactionTopicConfigs: Properties
```

`transactionTopicConfigs` requests the [TransactionStateManager](#txnManager) for the [transactionTopicConfigs](TransactionStateManager.md#transactionTopicConfigs)

---

`transactionTopicConfigs` is used when:

* `DefaultAutoTopicCreationManager` is requested to [creatableTopic](../DefaultAutoTopicCreationManager.md#creatableTopic) (for `__transaction_state` topic)

## <span id="onEndTransactionComplete"> onEndTransactionComplete

```scala
onEndTransactionComplete(
  txnIdAndPidEpoch: TransactionalIdAndProducerIdEpoch)(
  error: Errors): Unit
```

`onEndTransactionComplete` branches off per the `error` to print out a message to the logs.

---

`onEndTransactionComplete` is used when:

* `TransactionCoordinator` is requested to [start up](#startup) (and schedules the `transaction-abort` task)

### <span id="onEndTransactionComplete-NONE"> No Errors

For no errors, `onEndTransactionComplete` prints out the following INFO message to the logs:

```text
Completed rollback of ongoing transaction for transactionalId [transactionalId] due to timeout
```

### <span id="onEndTransactionComplete-Rollback-Cancelled"> Rollback Cancelled

For `INVALID_PRODUCER_ID_MAPPING`, `PRODUCER_FENCED`, `CONCURRENT_TRANSACTIONS`, `onEndTransactionComplete` prints out the following DEBUG message to the logs:

```text
Rollback of ongoing transaction for transactionalId [transactionalId] has been cancelled due to error [error]
```

### <span id="onEndTransactionComplete-Rollback-Failed"> Rollback Failed

For all other errors, `onEndTransactionComplete` prints out the following WARN message to the logs:

```text
Rollback of ongoing transaction for transactionalId [transactionalId] failed due to error [error]
```

## Logging

Enable `ALL` logging level for `kafka.coordinator.transaction.TransactionCoordinator` logger to see what happens inside.

Add the following line to `log4j.properties`:

```text
log4j.logger.kafka.coordinator.transaction.TransactionCoordinator=ALL
```

Refer to [Logging](../logging.md).
