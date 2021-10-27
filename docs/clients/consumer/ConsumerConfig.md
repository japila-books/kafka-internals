# ConsumerConfig

## <span id="auto.commit.interval.ms"><span id="AUTO_COMMIT_INTERVAL_MS_CONFIG"> auto.commit.interval.ms

## <span id="auto.offset.reset"><span id="AUTO_OFFSET_RESET_CONFIG"> auto.offset.reset

What to do when there is no initial offset in Kafka or if the current offset does not exist anymore on a broker (e.g. because that data has been deleted)

Default: `latest`

Supported values:

* `latest` - reset the offset to the latest offset
* `earliest` - reset the offset to the earliest offset
* `none` - throw exception to the consumer if no previous offset is found for the consumer's group

## <span id="check.crcs"><span id="CHECK_CRCS_CONFIG"> check.crcs

## <span id="client.rack"><span id="CLIENT_RACK_CONFIG"> client.rack

## <span id="enable.auto.commit"><span id="ENABLE_AUTO_COMMIT_CONFIG"> enable.auto.commit

Controls whether consumer offsets should be periodically committed in the background or not

Default: `true`

[kafka-console-consumer](../../tools/ConsoleConsumer.md) supports the property using `--consumer-property` or `--consumer.config` options.

Used when:

* `ConsumerConfig` is requested to [maybeOverrideEnableAutoCommit](#maybeOverrideEnableAutoCommit)

## <span id="fetch.max.bytes"><span id="FETCH_MAX_BYTES_CONFIG"> fetch.max.bytes

## <span id="fetch.max.wait.ms"><span id="FETCH_MAX_WAIT_MS_CONFIG"> fetch.max.wait.ms

## <span id="fetch.min.bytes"><span id="FETCH_MIN_BYTES_CONFIG"> fetch.min.bytes

## <span id="group.id"><span id="GROUP_ID_CONFIG"> group.id

See [CommonClientConfigs](../CommonClientConfigs.md#GROUP_ID_CONFIG)

## <span id="internal.throw.on.fetch.stable.offset.unsupported"><span id="THROW_ON_FETCH_STABLE_OFFSET_UNSUPPORTED"> internal.throw.on.fetch.stable.offset.unsupported

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

[kafka-console-consumer](../../tools/ConsoleConsumer.md) supports the property using `--isolation-level` option.

## <span id="max.partition.fetch.bytes"><span id="MAX_PARTITION_FETCH_BYTES_CONFIG"> max.partition.fetch.bytes

## <span id="max.poll.records"><span id="MAX_POLL_RECORDS_CONFIG"> max.poll.records

## <span id="request.timeout.ms"><span id="REQUEST_TIMEOUT_MS_CONFIG"> request.timeout.ms

## <span id="retry.backoff.ms"><span id="RETRY_BACKOFF_MS_CONFIG"> retry.backoff.ms

---

## <span id="maybeOverrideEnableAutoCommit"> maybeOverrideEnableAutoCommit

```java
boolean maybeOverrideEnableAutoCommit()
```

`maybeOverrideEnableAutoCommit` returns `false` when neither [group.id](../CommonClientConfigs.md#GROUP_ID_CONFIG) nor [enable.auto.commit](#ENABLE_AUTO_COMMIT_CONFIG) are specified. Otherwise, `maybeOverrideEnableAutoCommit` returns the value of [enable.auto.commit](#ENABLE_AUTO_COMMIT_CONFIG) configuration property.

`maybeOverrideEnableAutoCommit` throws an `InvalidConfigurationException` when no [group.id](../CommonClientConfigs.md#GROUP_ID_CONFIG) is given with [enable.auto.commit](#ENABLE_AUTO_COMMIT_CONFIG) enabled:

```text
enable.auto.commit cannot be set to true when default group id (null) is used.
```

`maybeOverrideEnableAutoCommit` is used when:

* `KafkaConsumer` is [created](KafkaConsumer.md) (and creates a [ConsumerCoordinator](ConsumerCoordinator.md#autoCommitEnabled))
