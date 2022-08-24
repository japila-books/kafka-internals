# TransactionStateManager

## Creating Instance

`TransactionStateManager` takes the following to be created:

* <span id="brokerId"> Broker ID
* <span id="scheduler"> `Scheduler`
* <span id="replicaManager"> [ReplicaManager](../ReplicaManager.md)
* <span id="config"> [TransactionConfig](TransactionConfig.md)
* <span id="time"> `Time`
* <span id="metrics"> `Metrics`

`TransactionStateManager` is created when:

* `TransactionCoordinator` utility is used to [create a TransactionCoordinator](TransactionCoordinator.md#apply)

## <span id="startup"> Starting Up

```scala
startup(
  retrieveTransactionTopicPartitionCount: () => Int,
  enableTransactionalIdExpiration: Boolean = true): Unit
```

`startup`...FIXME

`startup` is used when:

* `TransactionCoordinator` is requested to [start up](TransactionCoordinator.md#startup)

### <span id="enableTransactionalIdExpiration"> enableTransactionalIdExpiration

```scala
enableTransactionalIdExpiration(): Unit
```

`enableTransactionalIdExpiration`...FIXME

## <span id="appendTransactionToLog"> appendTransactionToLog

```scala
appendTransactionToLog(
  transactionalId: String,
  coordinatorEpoch: Int,
  newMetadata: TxnTransitMetadata,
  responseCallback: Errors => Unit,
  retryOnError: Errors => Boolean = _ => false): Unit
```

`appendTransactionToLog` generates the key and the value (of the record to represent the transaction in the topic) based on the given `transactionalId` and the `TxnTransitMetadata`, respectively.

`appendTransactionToLog`...FIXME

`appendTransactionToLog` requests the [ReplicaManager](#replicaManager) to [appendRecords](../ReplicaManager.md#appendRecords) (with `-1` acks, `internalTopicsAllowed` enabled annd `Coordinator` origin) and prints out the following TRACE message to the logs:

```text
Appending new metadata [newMetadata] for transaction id [transactionalId] with coordinator epoch [coordinatorEpoch] to the local transaction log
```

`appendTransactionToLog` is used when:

* `TransactionCoordinator` is requested to [handleInitProducerId](TransactionCoordinator.md#handleInitProducerId), [handleAddPartitionsToTransaction](TransactionCoordinator.md#handleAddPartitionsToTransaction), [endTransaction](TransactionCoordinator.md#endTransaction)
* `TransactionMarkerChannelManager` is requested to `tryAppendToLog`

## <span id="partitionFor"> partitionFor

```scala
partitionFor(
  transactionalId: String): Int
```

`partitionFor` calculates the partition for the given `transactionalId`.

`partitionFor` gets the absolute value of the `hashCode` of the `transactionalId` string modulo the number of partitions of the `__transaction_state` topic.

`partitionFor` is used when:

* `TransactionStateManager` is requested to [appendTransactionToLog](#appendTransactionToLog), [enableTransactionalIdExpiration](#enableTransactionalIdExpiration), [getAndMaybeAddTransactionState](#getAndMaybeAddTransactionState)
* `TransactionCoordinator` is requested to [handleInitProducerId](TransactionCoordinator.md#handleInitProducerId)
* `TransactionMarkerChannelManager` is requested to `addTxnMarkersToBrokerQueue`

## <span id="loadTransactionsForTxnTopicPartition"> loadTransactionsForTxnTopicPartition

```scala
loadTransactionsForTxnTopicPartition(
  partitionId: Int,
  coordinatorEpoch: Int,
  sendTxnMarkers: SendTxnMarkersCallback): Unit
```

`loadTransactionsForTxnTopicPartition`...FIXME

`loadTransactionsForTxnTopicPartition` is used when:

* `TransactionCoordinator` is requested to [onElection](TransactionCoordinator.md#onElection)

## <span id="removeTransactionsForTxnTopicPartition"> removeTransactionsForTxnTopicPartition

```scala
removeTransactionsForTxnTopicPartition(
  partitionId: Int): Unit
removeTransactionsForTxnTopicPartition(
  partitionId: Int,
  coordinatorEpoch: Int): Unit
```

`removeTransactionsForTxnTopicPartition`...FIXME

`removeTransactionsForTxnTopicPartition` is used when:

* `TransactionCoordinator` is requested to [onResignation](TransactionCoordinator.md#onResignation)

## <span id="transactionTopicConfigs"> transactionTopicConfigs

```scala
transactionTopicConfigs: Properties
```

Property Name | Property Value
---------|---------
 [cleanup.policy](../log/LogConfig.md#CleanupPolicyProp) | [compact](../log/LogConfig.md#Compact)
 [compression.type](../log/LogConfig.md#CompressionTypeProp) | `uncompressed`
 [min.insync.replicas](../log/LogConfig.md#MinInSyncReplicasProp) | [transaction.state.log.min.isr](TransactionConfig.md#transactionLogMinInsyncReplicas)
 [segment.bytes](../log/LogConfig.md#SegmentBytesProp) | [transaction.state.log.segment.bytes](TransactionConfig.md#transactionLogSegmentBytes)
 [unclean.leader.election.enable](../log/LogConfig.md#UncleanLeaderElectionEnableProp) | `false`

---

`transactionTopicConfigs` is used when:

* `TransactionCoordinator` is requested to [transactionTopicConfigs](TransactionCoordinator.md#transactionTopicConfigs)
