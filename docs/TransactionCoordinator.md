# TransactionCoordinator

## Creating Instance

`TransactionCoordinator` takes the following to be created:

* <span id="brokerId"> Broker Id
* <span id="txnConfig"> `TransactionConfig`
* <span id="scheduler"> `Scheduler`
* <span id="createProducerIdGenerator"> `createProducerIdGenerator` function (`() => ProducerIdGenerator`)
* <span id="txnManager"> `TransactionStateManager`
* <span id="txnMarkerChannelManager"> `TransactionMarkerChannelManager`
* <span id="time"> `Time`
* <span id="logContext"> `LogContext`

`TransactionCoordinator` is created (using [apply](#apply) factory) when:

* FIXME

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

`apply` creates a `TransactionConfig` with the following configuration properties:

* [transactional.id.expiration.ms](KafkaConfig.md#transactionalIdExpirationMs)
* [transaction.max.timeout.ms](KafkaConfig.md#transactionMaxTimeoutMs)
* [transaction.state.log.num.partitions](KafkaConfig.md#transactionTopicPartitions)
* [transaction.state.log.replication.factor](KafkaConfig.md#transactionTopicReplicationFactor)
* [transaction.state.log.segment.bytes](KafkaConfig.md#transactionTopicSegmentBytes)
* [transaction.state.log.load.buffer.size](KafkaConfig.md#transactionsLoadBufferSize)
* [transaction.state.log.min.isr](KafkaConfig.md#transactionTopicMinISR)
* [transaction.abort.timed.out.transaction.cleanup.interval.ms](KafkaConfig.md#transactionAbortTimedOutTransactionCleanupIntervalMs)
* [transaction.remove.expired.transaction.cleanup.interval.ms](KafkaConfig.md#transactionRemoveExpiredTransactionalIdCleanupIntervalMs)
* [request.timeout.ms](KafkaConfig.md#requestTimeoutMs)

`apply` creates a `TransactionStateManager` (with the [brokerId](KafkaConfig.md#brokerId) and the other Kafka services).

`apply` creates a `LogContext` that uses the following log prefix (with the [brokerId](KafkaConfig.md#brokerId)):

```text
[TransactionCoordinator id=[brokerId]]
```

`apply` creates a `TransactionMarkerChannelManager`.

In the end, `apply` creates a [TransactionCoordinator](#creating-instance).

`apply` is used when:

* `BrokerServer` is requested to [startup](BrokerServer.md#startup)
* `KafkaServer` is requested to [startup](KafkaServer.md#startup)

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
on partition __transaction_state-[partition]}
```

In the end, `handleInitProducerId` requests the [TransactionStateManager](#txnManager) to [appendTransactionToLog](TransactionStateManager.md#appendTransactionToLog).

`handleInitProducerId` is used when:

* `KafkaApis` is requested to [handleInitProducerIdRequest](KafkaApis.md#handleInitProducerIdRequest)
