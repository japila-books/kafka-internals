# TransactionStateManager

## Creating Instance

`TransactionStateManager` takes the following to be created:

* <span id="brokerId"> Broker ID
* <span id="scheduler"> `Scheduler`
* <span id="replicaManager"> [ReplicaManager](ReplicaManager.md)
* <span id="config"> `TransactionConfig`
* <span id="time"> `Time`
* <span id="metrics"> `Metrics`

`TransactionStateManager` is created when:

* `TransactionCoordinator` utility is used to [create a TransactionCoordinator](TransactionCoordinator.md#apply)

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

`appendTransactionToLog` requests the [ReplicaManager](#replicaManager) to [appendRecords](ReplicaManager.md#appendRecords) (with `-1` acks, `internalTopicsAllowed` enabled annd `Coordinator` origin) and prints out the following TRACE message to the logs:

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
