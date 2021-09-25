# Fetcher

## Creating Instance

`Fetcher` takes the following to be created:

* <span id="logContext"> `LogContext`
* <span id="client"> `ConsumerNetworkClient`
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
* <span id="subscriptions"> `SubscriptionState`
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

* [sendFetches](#sendFetches)
* [fetchOffsetsByTimes](#fetchOffsetsByTimes)
* [fetchRecords](#fetchRecords)
* [sendListOffsetRequest](#sendListOffsetRequest)
* [prepareFetchRequests](#prepareFetchRequests)

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

## <span id="fetchedRecords"> fetchedRecords

```java
Map<TopicPartition, List<ConsumerRecord<K, V>>> fetchedRecords()
```

`fetchedRecords`...FIXME

`fetchedRecords` is used when:

* `KafkaConsumer` is requested to [pollForFetches](KafkaConsumer.md#pollForFetches)

### <span id="fetchRecords"> fetchRecords

```java
List<ConsumerRecord<K, V>> fetchRecords(
  CompletedFetch completedFetch,
  int maxRecords)
```

`fetchRecords`...FIXME

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

* `Fetcher` is requested to [resetOffsetsAsync](#resetOffsetsAsync) and [sendListOffsetsRequests](#sendListOffsetsRequests)
