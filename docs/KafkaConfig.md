# KafkaConfig

## <span id="transactionalIdExpirationMs"><span id="TransactionalIdExpirationMsProp"><span id="transactional.id.expiration.ms"> transactional.id.expiration.ms

## <span id="transactionMaxTimeoutMs"><span id="TransactionsMaxTimeoutMsProp"><span id="transaction.max.timeout.ms"> transaction.max.timeout.ms

## <span id="transactionTopicPartitions"><span id="TransactionsTopicPartitionsProp"><span id="transaction.state.log.num.partitions"> transaction.state.log.num.partitions

The number of partitions for the [transaction topic](transactions/index.md#TRANSACTION_STATE_TOPIC_NAME)

Default: 50

Must be at least 1

## <span id="transactionTopicReplicationFactor"><span id="TransactionsTopicReplicationFactorProp"><span id="transaction.state.log.replication.factor"> transaction.state.log.replication.factor

## <span id="transactionTopicSegmentBytes"><span id="TransactionsTopicSegmentBytesProp"><span id="transaction.state.log.segment.bytes"> transaction.state.log.segment.bytes

## <span id="transactionsLoadBufferSize"><span id="TransactionsLoadBufferSizeProp"><span id="transaction.state.log.load.buffer.size"> transaction.state.log.load.buffer.size

## <span id="transactionTopicMinISR"><span id="TransactionsTopicMinISRProp"><span id="transaction.state.log.min.isr"> transaction.state.log.min.isr

## <span id="transactionAbortTimedOutTransactionCleanupIntervalMs"><span id="TransactionsAbortTimedOutTransactionCleanupIntervalMsProp"><span id="transaction.abort.timed.out.transaction.cleanup.interval.ms"> transaction.abort.timed.out.transaction.cleanup.interval.ms

## <span id="transactionRemoveExpiredTransactionalIdCleanupIntervalMs"><span id="TransactionsRemoveExpiredTransactionalIdCleanupIntervalMsProp"><span id="transaction.remove.expired.transaction.cleanup.interval.ms"> transaction.remove.expired.transaction.cleanup.interval.ms

## <span id="process.roles"><span id="ProcessRolesProp"><span id="parseProcessRoles"><span id="processRoles"><span id="usesSelfManagedQuorum"> process.roles

A comma-separated list of the roles that this process plays in a Kafka cluster:

Supported values:

* `broker`
* `controller`

Default: (empty)

1. When empty, the process requires Zookeeper (runs with Zookeeper).
1. Only applicable for clusters in KRaft (Kafka Raft) mode
1. If used, [controller.quorum.voters](raft/RaftConfig.md#QUORUM_VOTERS_CONFIG) must contain a parseable set of voters
1. [advertised.listeners](#AdvertisedListenersProp) config must not contain KRaft controller listeners from [controller.listener.names](#ControllerListenerNamesProp) when `process.roles` contains `broker` role because Kafka clients that send requests via advertised listeners do not send requests to KRaft controllers -- they only send requests to KRaft brokers
1. If `process.roles` contains `controller` role, the [node.id](KafkaConfig.md#NodeIdProp) must be included in the set of voters [controller.quorum.voters](raft/RaftConfig.md#QUORUM_VOTERS_CONFIG)
1. If `process.roles` contains just the `broker` role, the [node.id](KafkaConfig.md#NodeIdProp) must not be included in the set of voters [controller.quorum.voters](raft/RaftConfig.md#QUORUM_VOTERS_CONFIG)
1. If [controller.listener.names](#ControllerListenerNamesProp) has multiple entries; only the first will be used when `process.roles` is `broker`
1. The advertised listeners ([advertised.listeners](#AdvertisedListenersProp) or [listeners](#ListenersProp)) config must only contain KRaft controller listeners from [controller.listener.names](#ControllerListenerNamesProp) when `process.roles` is `controller`

## <span id="requestTimeoutMs"><span id="RequestTimeoutMsProp"><span id="request.timeout.ms"> request.timeout.ms

[request.timeout.ms](clients/CommonClientConfigs.md#REQUEST_TIMEOUT_MS_CONFIG)

## Utilities

### <span id="requiresZookeeper"> requiresZookeeper

```scala
requiresZookeeper: Boolean
```

`requiresZookeeper` is `true` when [process.roles](#processRoles) is empty.
