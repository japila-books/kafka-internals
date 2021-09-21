# TransactionConfig

`TransactionConfig` holds the values of the transactional configuration properties.

## <span id="transactionalIdExpirationMs"> transactional.id.expiration.ms

[transactional.id.expiration.ms](../KafkaConfig.md#transactionalIdExpirationMs)

Default: 7 days

## <span id="transactionMaxTimeoutMs"> transaction.max.timeout.ms

[transaction.max.timeout.ms](../KafkaConfig.md#transactionMaxTimeoutMs)

Default: 15 minutes

## <span id="transactionLogNumPartitions"> transaction.state.log.num.partitions

[transaction.state.log.num.partitions](../KafkaConfig.md#transactionTopicPartitions)

Default: 50

## <span id="transactionLogReplicationFactor"> transaction.state.log.replication.factor

[transaction.state.log.replication.factor](../KafkaConfig.md#transactionTopicReplicationFactor)

Default: 3

## <span id="transactionLogSegmentBytes"> transaction.state.log.segment.bytes

[transaction.state.log.segment.bytes](../KafkaConfig.md#transactionTopicSegmentBytes)

Default: 100 * 1024 * 1024

## <span id="transactionLogLoadBufferSize"> transaction.state.log.load.buffer.size

[transaction.state.log.load.buffer.size](../KafkaConfig.md#transactionsLoadBufferSize)

Default: 5 * 1024 * 1024

## <span id="transactionLogMinInsyncReplicas"> transaction.state.log.min.isr

[transaction.state.log.min.isr](../KafkaConfig.md#transactionTopicMinISR)

Default: 2

## <span id="abortTimedOutTransactionsIntervalMs"> transaction.abort.timed.out.transaction.cleanup.interval.ms

[transaction.abort.timed.out.transaction.cleanup.interval.ms](../KafkaConfig.md#transactionAbortTimedOutTransactionCleanupIntervalMs)

Default: 10 seconds

## <span id="removeExpiredTransactionalIdsIntervalMs"> transaction.remove.expired.transaction.cleanup.interval.ms

[transaction.remove.expired.transaction.cleanup.interval.ms](../KafkaConfig.md#transactionRemoveExpiredTransactionalIdCleanupIntervalMs)

Default: 1 hour

## <span id="requestTimeoutMs"> request.timeout.ms

[request.timeout.ms](../KafkaConfig.md#requestTimeoutMs)

Default: 30000
