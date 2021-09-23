# ProducerConfig

## <span id="acks"><span id="ACKS_CONFIG"> acks

## <span id="batch.size"><span id="BATCH_SIZE_CONFIG"> batch.size

The buffer size allocated for a partition. When records are received (which are smaller than this size) [KafkaProducer](KafkaProducer.md) will attempt to optimistically group them together until this size is reached.

Default: `16384`

Must be at least 0

Related to:

* [linger.ms](#linger.ms)
* `max-partition-memory-bytes` (`ConsoleProducer`)

Used when:

* `KafkaProducer` is [created](KafkaProducer.md#creating-instance) (to create a [RecordAccumulator](KafkaProducer.md#accumulator) and an accompanying [BufferPool](RecordAccumulator.md#bufferPool))
* `KafkaLog4jAppender` is requested to `activateOptions`

## <span id="enable.idempotence"><span id="ENABLE_IDEMPOTENCE_CONFIG"> enable.idempotence

Default: `false`

Used when:

* `KafkaProducer` is requested to [configureTransactionState](KafkaProducer.md#configureTransactionState)
* `ProducerConfig` is requested to [maybeOverrideEnableIdempotence](#maybeOverrideEnableIdempotence) and [idempotenceEnabled](#idempotenceEnabled)

## <span id="linger.ms"><span id="LINGER_MS_CONFIG"> linger.ms

## <span id="max.block.ms"><span id="MAX_BLOCK_MS_CONFIG"> max.block.ms

## <span id="max.in.flight.requests.per.connection"><span id="MAX_IN_FLIGHT_REQUESTS_PER_CONNECTION"> max.in.flight.requests.per.connection

The maximum number of unacknowledged requests the client will send on a single connection before blocking.
Note that if this setting is set to be greater than 1 and there are failed sends, there is a risk of message re-ordering due to retries (i.e., if retries are enabled).

Default: `5`

Must be at least 1

Related to:

* [enable.idempotence](#enable.idempotence)
* [retries](#retries)

Used when:

* `KafkaProducer` is requested to [configureInflightRequests](KafkaProducer.md#configureInflightRequests)

## <span id="partitioner.class"><span id="PARTITIONER_CLASS_CONFIG"> partitioner.class

The class of the [Partitioner](Partitioner.md) for a [KafkaProducer](KafkaProducer.md#partitioner)

Default: [DefaultPartitioner](DefaultPartitioner.md)

## <span id="retries"><span id="RETRIES_CONFIG"> retries

## <span id="retry.backoff.ms"><span id="RETRY_BACKOFF_MS_CONFIG"> retry.backoff.ms

[retry.backoff.ms](../CommonClientConfigs.md#RETRY_BACKOFF_MS_CONFIG)

## <span id="transactional.id"><span id="TRANSACTIONAL_ID_CONFIG"> transactional.id

The ID of a [KafkaProducer](KafkaProducer.md) for transactional delivery

Default: (undefined)

This enables reliability semantics which span multiple producer sessions since it allows the client to guarantee that transactions using the same `transactional.id` have been completed prior to starting any new transactions.

With no `transactional.id`, a producer is limited to idempotent delivery.

When configured, [enable.idempotence](#enable.idempotence) is [implied](#maybeOverrideEnableIdempotence) (and configured when `KafkaProducer` is [created](KafkaProducer.md#configureTransactionState)).

With `transactional.id`, `KafkaProducer` uses a modified [client.id](#client.id) (that [includes the ID](#maybeOverrideClientId)).

Note that, by default, transactions require a cluster of at least three brokers which is the recommended setting for production; for development you can change this, by adjusting broker setting [transaction.state.log.replication.factor](#transaction.state.log.replication.factor).

`transactional.id` is required for the [transactional methods](KafkaProducer.md#throwIfNoTransactionManager).

Used when:

* [KafkaProducer](KafkaProducer.md) prints out log messages (with the transactional ID included in the log prefix)
* `KafkaProducer` is [created](KafkaProducer.md#configureTransactionState) (and creates a [TransactionManager](TransactionManager.md))

## <span id="transaction.state.log.replication.factor"> transaction.state.log.replication.factor

## <span id="transaction.timeout.ms"><span id="TRANSACTION_TIMEOUT_CONFIG"> transaction.timeout.ms

## <span id="idempotenceEnabled"> idempotenceEnabled

```java
boolean idempotenceEnabled()
```

`idempotenceEnabled` is enabled (`true`) when one of the following holds:

1. [transactional.id](#transactional.id) is defined
1. [enable.idempotence](#enable.idempotence) is enabled

`idempotenceEnabled` throws a `ConfigException` when [enable.idempotence](#enable.idempotence) is disabled but [transactional.id](#transactional.id) is defined:

```text
Cannot set a transactional.id without also enabling idempotence.
```

`idempotenceEnabled` is used when:

* `KafkaProducer` is [created](KafkaProducer.md#creating-instance) (and requested to [configureTransactionState](KafkaProducer.md#configureTransactionState), [configureInflightRequests](KafkaProducer.md#configureInflightRequests), [configureAcks](KafkaProducer.md#configureAcks))
* `ProducerConfig` is requested to [maybeOverrideAcksAndRetries](#maybeOverrideAcksAndRetries)

## <span id="postProcessParsedConfig"> postProcessParsedConfig

```java
Map<String, Object> postProcessParsedConfig(
  Map<String, Object> parsedValues)
```

`postProcessParsedConfig` [maybeOverrideEnableIdempotence](#maybeOverrideEnableIdempotence).
`postProcessParsedConfig` [maybeOverrideClientId](#maybeOverrideClientId).
`postProcessParsedConfig` [maybeOverrideAcksAndRetries](#maybeOverrideAcksAndRetries).

`postProcessParsedConfig` is part of the [AbstractConfig](../../AbstractConfig.md#postProcessParsedConfig) abstraction.

### <span id="maybeOverrideClientId"> maybeOverrideClientId

`maybeOverrideAcksAndRetries` overrides [client.id](#client.id) configuration property unless already defined.

The new value uses [transactional.id](#transactional.id) (if defined) or the next available ID with the `producer-` prefix.

### <span id="maybeOverrideAcksAndRetries"> maybeOverrideAcksAndRetries

```java
void maybeOverrideAcksAndRetries(
  Map<String, Object> configs)
```

`maybeOverrideAcksAndRetries`...FIXME

### <span id="maybeOverrideEnableIdempotence"> maybeOverrideEnableIdempotence

```java
void maybeOverrideEnableIdempotence(
  Map<String, Object> configs)
```

`maybeOverrideEnableIdempotence` sets [enable.idempotence](#enable.idempotence) configuration property to `true` when [transactional.id](#transactional.id) is defined with no [enable.idempotence](#enable.idempotence).
