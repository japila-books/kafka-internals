# Fetcher

`Fetcher<K, V>` is used by [KafkaConsumer](KafkaConsumer.md#fetcher) for [fetching records](#fetchedRecords).

## Creating Instance

`Fetcher` takes the following to be created:

* <span id="logContext"> `LogContext`
* <span id="client"> [ConsumerNetworkClient](ConsumerNetworkClient.md)
* <span id="minBytes"> [fetch.min.bytes](ConsumerConfig.md#FETCH_MIN_BYTES_CONFIG)
* <span id="maxBytes"> [fetch.max.bytes](ConsumerConfig.md#FETCH_MAX_BYTES_CONFIG)
* <span id="maxWaitMs"> [fetch.max.wait.ms](ConsumerConfig.md#FETCH_MAX_WAIT_MS_CONFIG)
* <span id="fetchSize"> [max.partition.fetch.bytes](ConsumerConfig.md#MAX_PARTITION_FETCH_BYTES_CONFIG)
* <span id="maxPollRecords"> [max.poll.records](ConsumerConfig.md#MAX_POLL_RECORDS_CONFIG)
* <span id="checkCrcs"> [check.crcs](ConsumerConfig.md#CHECK_CRCS_CONFIG)
* <span id="clientRackId"> [client.rack](ConsumerConfig.md#CLIENT_RACK_CONFIG)
* <span id="keyDeserializer"> Key `Deserializer`
* <span id="valueDeserializer"> Value `Deserializer`
* <span id="metadata"> `ConsumerMetadata`
* <span id="subscriptions"> [SubscriptionState](SubscriptionState.md)
* <span id="metrics"> `Metrics`
* <span id="metricsRegistry"> `FetcherMetricsRegistry`
* <span id="time"> `Time`
* <span id="retryBackoffMs"> [retry.backoff.ms](ConsumerConfig.md#RETRY_BACKOFF_MS_CONFIG)
* <span id="requestTimeoutMs"> [request.timeout.ms](ConsumerConfig.md#REQUEST_TIMEOUT_MS_CONFIG)
* [IsolationLevel](#isolationLevel)
* <span id="apiVersions"> `ApiVersions`

`Fetcher` is created along with [KafkaConsumer](KafkaConsumer.md#fetcher).

## <span id="isolationLevel"> IsolationLevel

`Fetcher` is given an `IsolationLevel` when [created](#creating-instance) (based on [isolation.level](ConsumerConfig.md#ISOLATION_LEVEL_CONFIG) configuration property)

`Fetcher` uses the `IsolationLevel` for the following:

* [sendFetches](#sendFetches) (and [prepareFetchRequests](#prepareFetchRequests))
* [fetchOffsetsByTimes](#fetchOffsetsByTimes)
* [fetchRecords](#fetchRecords)
* [sendListOffsetRequest](#sendListOffsetRequest)

## <span id="sendFetches"> sendFetches

```java
int sendFetches()
```

`sendFetches`...FIXME

`sendFetches` is used when:

* `KafkaConsumer` is requested to [poll](KafkaConsumer.md#poll) (and [pollForFetches](KafkaConsumer.md#pollForFetches))

### <span id="prepareFetchRequests"> prepareFetchRequests

```java
Map<Node, FetchSessionHandler.FetchRequestData> prepareFetchRequests()
```

`prepareFetchRequests`...FIXME

### <span id="selectReadReplica"> Preferred Read Replica

```java
Node selectReadReplica(
  TopicPartition partition,
  Node leaderReplica,
  long currentTimeMs)
```

`selectReadReplica` requests the [SubscriptionState](#subscriptions) for the [preferredReadReplica](SubscriptionState.md#preferredReadReplica) of the given `TopicPartition`.

## <span id="offsetsForTimes"> offsetsForTimes

```java
Map<TopicPartition, OffsetAndTimestamp> offsetsForTimes(
  Map<TopicPartition, Long> timestampsToSearch,
  Timer timer)
```

`offsetsForTimes`...FIXME

`offsetsForTimes` is used when:

* `KafkaConsumer` is requested to [offsetsForTimes](KafkaConsumer.md#offsetsForTimes)

## <span id="beginningOffsets"> beginningOffsets

```java
Map<TopicPartition, Long> beginningOffsets(
  Collection<TopicPartition> partitions,
  Timer timer)
```

`beginningOffsets`...FIXME

`beginningOffsets` is used when:

* `KafkaConsumer` is requested to [beginningOffsets](KafkaConsumer.md#beginningOffsets)

## <span id="endOffsets"> endOffsets

```java
Map<TopicPartition, Long> endOffsets(
  Collection<TopicPartition> partitions,
  Timer timer)
```

`endOffsets`...FIXME

`endOffsets` is used when:

* `KafkaConsumer` is requested to [endOffsets](KafkaConsumer.md#endOffsets) and [currentLag](KafkaConsumer.md#currentLag)

## <span id="beginningOrEndOffset"> beginningOrEndOffset

```java
Map<TopicPartition, Long> beginningOrEndOffset(
  Collection<TopicPartition> partitions,
  long timestamp,
  Timer timer)
```

`beginningOrEndOffset`...FIXME

`beginningOrEndOffset` is used when:

* `Fetcher` is requested to [beginningOffsets](#beginningOffsets) and [endOffsets](#endOffsets)

## <span id="fetchOffsetsByTimes"> fetchOffsetsByTimes

```java
ListOffsetResult fetchOffsetsByTimes(
  Map<TopicPartition, Long> timestampsToSearch,
  Timer timer,
  boolean requireTimestamps)
```

`fetchOffsetsByTimes`...FIXME

`fetchOffsetsByTimes` is used when:

* `Fetcher` is requested to [offsetsForTimes](#offsetsForTimes) and [beginningOrEndOffset](#beginningOrEndOffset)

### <span id="sendListOffsetsRequests"> sendListOffsetsRequests

```java
RequestFuture<ListOffsetResult> sendListOffsetsRequests(
  Map<TopicPartition, Long> timestampsToSearch,
  boolean requireTimestamps)
```

`sendListOffsetsRequests`...FIXME

## <span id="fetchedRecords"> Fetched Records

```java
Map<TopicPartition, List<ConsumerRecord<K, V>>> fetchedRecords()
```

`fetchedRecords` returns up to [max.poll.records](ConsumerConfig.md#max.poll.records) number of records from the [CompletedFetch Queue](#completedFetches).

---

For `nextInLineFetch` unintialized or consumed already, `fetchedRecords` takes a peek at a `CompletedFetch` collection of records (in the [CompletedFetch Queue](#completedFetches)). If uninitialized, `fetchedRecords` [initializeCompletedFetch](#initializeCompletedFetch) with the records. `fetchedRecords` saves the `CompletedFetch` records to the [nextInLineFetch](#nextInLineFetch) internal registry. `fetchedRecords` takes the `CompletedFetch` collection of records out (off the [CompletedFetch Queue](#completedFetches)).

For the partition of the [nextInLineFetch](#nextInLineFetch) collection of records [paused](SubscriptionState.md#isPaused), `fetchedRecords` prints out the following DEBUG message to the logs and `null`s the [nextInLineFetch](#nextInLineFetch) registry.

```text
Skipping fetching records for assigned partition [p] because it is paused
```

For all the other cases, `fetchedRecords` [fetches the records](#fetchRecords) out of the [nextInLineFetch](#nextInLineFetch) collection of records (up to the number of records left to fetch).

In the end, `fetchedRecords` returns the `ConsumerRecord`s per `TopicPartition` (out of the [CompletedFetch Queue](#completedFetches)).

`fetchedRecords` is used when:

* `KafkaConsumer` is requested to [pollForFetches](KafkaConsumer.md#pollForFetches)

### <span id="fetchRecords"> fetchRecords

```java
List<ConsumerRecord<K, V>> fetchRecords(
  CompletedFetch completedFetch,
  int maxRecords)
```

`fetchRecords`...FIXME

### <span id="initializeCompletedFetch"> initializeCompletedFetch

```java
CompletedFetch initializeCompletedFetch(
  CompletedFetch nextCompletedFetch)
```

`initializeCompletedFetch` returns the given `CompletedFetch` if there were no errors. `initializeCompletedFetch` updates the [SubscriptionState](#subscriptions) with the current metadata about the partition.

---

`initializeCompletedFetch` takes the partition, the `PartitionData` and the fetch offset from the given `CompletedFetch`.

`initializeCompletedFetch` prints out the following TRACE message to the logs:

```text
Preparing to read [n] bytes of data for partition [p] with offset [o]
```

`initializeCompletedFetch` takes the `RecordBatch`es from the `PartitionData`.

With a high watermark given, `initializeCompletedFetch` prints out the following TRACE message to the logs and requests the [SubscriptionState](#subscriptions) to [updateHighWatermark](SubscriptionState.md#updateHighWatermark) for the partition.

```text
Updating high watermark for partition [p] to [highWatermark]
```

With a log start offset given, `initializeCompletedFetch` prints out the following TRACE message to the logs and requests the [SubscriptionState](#subscriptions) to [updateLogStartOffset](SubscriptionState.md#updateLogStartOffset) for the partition.

```text
Updating log start offset for partition [p] to [logStartOffset]
```

With a last stable offset given, `initializeCompletedFetch` prints out the following TRACE message to the logs and requests the [SubscriptionState](#subscriptions) to [updateLastStableOffset](SubscriptionState.md#updateLastStableOffset) for the partition.

```text
Updating last stable offset for partition [p] to [lastStableOffset]
```

With a preferred read replica given, `initializeCompletedFetch` prints out the following DEBUG message to the logs and requests the [SubscriptionState](#subscriptions) to [updatePreferredReadReplica](SubscriptionState.md#updatePreferredReadReplica) for the partition.

```text
Updating preferred read replica for partition [p] to [preferredReadReplica] set to expire at [expireTimeMs]
```

For errors like `NOT_LEADER_OR_FOLLOWER`, `REPLICA_NOT_AVAILABLE`, `FENCED_LEADER_EPOCH`, `KAFKA_STORAGE_ERROR`, `OFFSET_NOT_AVAILABLE`, `initializeCompletedFetch` prints out the following DEBUG message to the logs and requests the [ConsumerMetadata](#metadata) to `requestUpdate`.

```text
Error in fetch for partition [p]: [exceptionName]
```

For `OFFSET_OUT_OF_RANGE` error, `initializeCompletedFetch` requests the [SubscriptionState](#subscriptions) to [clearPreferredReadReplica](SubscriptionState.md#clearPreferredReadReplica) for the partition. With no preferred read replica, it is assumed that the fetch came from the leader.

## <span id="resetOffsetsIfNeeded"> resetOffsetsIfNeeded

```java
void resetOffsetsIfNeeded()
```

`resetOffsetsIfNeeded`...FIXME

`resetOffsetsIfNeeded` is used when:

* `KafkaConsumer` is requested to [updateFetchPositions](KafkaConsumer.md#updateFetchPositions)

### <span id="resetOffsetsAsync"> resetOffsetsAsync

```java
void resetOffsetsAsync(
  Map<TopicPartition, Long> partitionResetTimestamps)
```

`resetOffsetsAsync`...FIXME

## <span id="sendListOffsetRequest"> sendListOffsetRequest

```java
RequestFuture<ListOffsetResult> sendListOffsetRequest(
  Node node,
  Map<TopicPartition, ListOffsetsPartition> timestampsToSearch,
  boolean requireTimestamp)
```

`sendListOffsetRequest`...FIXME

`sendListOffsetRequest` is used when:

* `Fetcher` is requested to [resetOffsetsIfNeeded](#resetOffsetsIfNeeded) (via [resetOffsetsAsync](#resetOffsetsAsync)) and [fetchOffsetsByTimes](#fetchOffsetsByTimes) (via [sendListOffsetsRequests](#sendListOffsetsRequests))

## <span id="clearBufferedDataForUnassignedTopics"> clearBufferedDataForUnassignedTopics

```java
void clearBufferedDataForUnassignedTopics(
  Collection<String> assignedTopics)
```

`clearBufferedDataForUnassignedTopics`...FIXME

`clearBufferedDataForUnassignedTopics` is used when:

* `KafkaConsumer` is requested to [subscribe](KafkaConsumer.md#subscribe)

## <span id="completedFetches"> CompletedFetch Queue

`Fetcher` creates an empty `ConcurrentLinkedQueue` ([Java]({{ java.api }}/java/util/concurrent/ConcurrentLinkedQueue.html)) of `CompletedFetch`es when [created](#creating-instance).

New `CompletedFetch`es (one per partition) are added to the queue in [sendFetches](#sendFetches) (on a successful receipt of response from a Kafka cluster).

## Logging

Enable `ALL` logging level for `org.apache.kafka.clients.consumer.internals.Fetcher` logger to see what happens inside.

Add the following line to `log4j.properties`:

```text
log4j.logger.org.apache.kafka.clients.consumer.internals.Fetcher=ALL
```

Refer to [Logging](../../logging.md).
