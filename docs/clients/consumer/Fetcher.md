# Fetcher

## Creating Instance

`Fetcher` takes the following to be created:

* <span id="logContext"> `LogContext`
* <span id="client"> `ConsumerNetworkClient`
* <span id="minBytes"> minBytes
* <span id="maxBytes"> maxBytes
* <span id="maxWaitMs"> maxWaitMs
* <span id="fetchSize"> fetchSize
* <span id="maxPollRecords"> maxPollRecords
* <span id="checkCrcs"> checkCrcs
* <span id="clientRackId"> clientRackId
* <span id="keyDeserializer"> Key `Deserializer`
* <span id="valueDeserializer"> Value `Deserializer`
* <span id="metadata"> `ConsumerMetadata`
* <span id="subscriptions"> `SubscriptionState`
* <span id="metrics"> `Metrics`
* <span id="metricsRegistry"> `FetcherMetricsRegistry`
* <span id="time"> `Time`
* <span id="retryBackoffMs"> retryBackoffMs
* <span id="requestTimeoutMs"> requestTimeoutMs
* <span id="isolationLevel"> `IsolationLevel`
* <span id="apiVersions"> `ApiVersions`

`Fetcher` is created along with [KafkaConsumer](KafkaConsumer.md#fetcher).

## <span id="sendFetches"> sendFetches

```java
int sendFetches()
```

`sendFetches`...FIXME

`sendFetches` is used when:

* `KafkaConsumer` is requested to [poll](KafkaConsumer.md#poll) (and [pollForFetches](KafkaConsumer.md#pollForFetches))
