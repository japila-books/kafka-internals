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

## <span id="process.roles"><span id="ProcessRolesProp"><span id="parseProcessRoles"><span id="processRoles"><span id="requiresZookeeper"><span id="usesSelfManagedQuorum"> process.roles

The roles that this process plays: `broker`, `controller`, or `broker,controller`. When undefined or empty the cluster runs with Zookeeper.

Default: (empty)

Supported values:

* `broker`
* `controller`

This configuration is only applicable for clusters in KRaft (Kafka Raft) mode (instead of ZooKeeper).

## <span id="requestTimeoutMs"><span id="RequestTimeoutMsProp"><span id="request.timeout.ms"> request.timeout.ms

[request.timeout.ms](clients/CommonClientConfigs.md#REQUEST_TIMEOUT_MS_CONFIG)
