# ProducerConfig

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

## <span id="transactional.id"><span id="TRANSACTIONAL_ID_CONFIG"> transactional.id

The `TransactionalId` to use for transactional delivery. This enables reliability semantics which span multiple producer sessions since it allows the client to guarantee that transactions using the same TransactionalId have been completed prior to starting any new transactions. If no TransactionalId is provided, then the producer is limited to idempotent delivery. If a TransactionalId is configured, [enable.idempotence](#enable.idempotence) is implied.

Default: (empty) (transactions cannot be used)

Note that, by default, transactions require a cluster of at least three brokers which is the recommended setting for production; for development you can change this, by adjusting broker setting [transaction.state.log.replication.factor](#transaction.state.log.replication.factor).

## <span id="transaction.state.log.replication.factor"> transaction.state.log.replication.factor

## <span id="idempotenceEnabled"> idempotenceEnabled

```java
boolean idempotenceEnabled()
```

`idempotenceEnabled`...FIXME

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

`postProcessParsedConfig` is part of the [AbstractConfig](../AbstractConfig.md#postProcessParsedConfig) abstraction.

### <span id="maybeOverrideClientId"> maybeOverrideClientId

`maybeOverrideAcksAndRetries` uses [transactional.id](#transactional.id) (if defined) or the next available ID for an ID with `producer-` prefix for [client.id](../CommonClientConfigs.md#client.id) unless already defined.

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

`maybeOverrideEnableIdempotence`...FIXME
