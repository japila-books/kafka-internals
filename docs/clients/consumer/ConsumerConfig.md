# ConsumerConfig

## <span id="check.crcs"><span id="CHECK_CRCS_CONFIG"> check.crcs

## <span id="client.rack"><span id="CLIENT_RACK_CONFIG"> client.rack

## <span id="fetch.max.bytes"><span id="FETCH_MAX_BYTES_CONFIG"> fetch.max.bytes

## <span id="fetch.max.wait.ms"><span id="FETCH_MAX_WAIT_MS_CONFIG"> fetch.max.wait.ms

## <span id="fetch.min.bytes"><span id="FETCH_MIN_BYTES_CONFIG"> fetch.min.bytes

## <span id="group.id"><span id="GROUP_ID_CONFIG"> group.id

A unique identifier of the consumer group a consumer belongs to. Required if the consumer uses either the group management functionality by using [Consumer.subscribe](Consumer.md#subscribe) or the Kafka-based offset management strategy.

Default: (undefined)

## <span id="isolation.level"><span id="ISOLATION_LEVEL_CONFIG"> isolation.level

Controls how [KafkaConsumer](KafkaConsumer.md#isolationLevel) should read messages written transactionally

Default: `read_uncommitted`

Supported values:

* `read_uncommitted` - [Consumer.poll()](Consumer.md#poll) will only return transactional messages which have been committed (filtering out transactional messages which are not committed).

* `read_committed` - [Consumer.poll()](Consumer.md#poll) will return all messages, even transactional messages which have been not committed yet or even aborted.

Non-transactional messages will be returned unconditionally in either mode.

Messages will always be returned in offset order. Hence, in `read_committed` mode, `consumer.poll()` will only return messages up to the last stable offset (LSO), which is the one less than the offset of the first open transaction.

In particular any messages appearing after messages belonging to ongoing transactions will be withheld until the relevant transaction has been completed.

As a result, `read_committed` consumers will not be able to read up to the high watermark when there are in-flight transactions.

Further, when in `read_committed` the `seekToEnd` method will return the last stable offset.

`kafka-console-consumer` supports setting the property using `--isolation-level` option.

## <span id="max.partition.fetch.bytes"><span id="MAX_PARTITION_FETCH_BYTES_CONFIG"> max.partition.fetch.bytes

## <span id="max.poll.records"><span id="MAX_POLL_RECORDS_CONFIG"> max.poll.records

## <span id="request.timeout.ms"><span id="REQUEST_TIMEOUT_MS_CONFIG"> request.timeout.ms

## <span id="retry.backoff.ms"><span id="RETRY_BACKOFF_MS_CONFIG"> retry.backoff.ms
