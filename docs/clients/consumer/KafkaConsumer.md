# KafkaConsumer

`KafkaConsumer` is a [Consumer](Consumer.md).

## Creating Instance

`KafkaConsumer` takes the following to be created:

* <span id="config"><span id="configs"><span id="properties"> Configuration ([ConsumerConfig](ConsumerConfig.md) or `Map<String, Object>` or `Properties`)
* <span id="keyDeserializer"> `Deserializer<K>`
* <span id="valueDeserializer"> `Deserializer<V>`

## <span id="groupId"> Group ID

`KafkaConsumer` can be given a group ID using [group.id](ConsumerConfig.md#GROUP_ID_CONFIG) (indirectly in the [config](#config)) configuration property when [created](#creating-instance).

## <span id="isolationLevel"> IsolationLevel

`KafkaConsumer` can be given an `IsolationLevel` using [isolation.level](ConsumerConfig.md#ISOLATION_LEVEL_CONFIG) configuration property (indirectly in the [config](#config)) when [created](#creating-instance).

`KafkaConsumer` uses the `IsolationLevel` for the following:

* Creating a [Fetcher](Fetcher.md#isolationLevel) (when [created](#creating-instance))
* [currentLag](#currentLag)

## <span id="fetcher"> Fetcher

`KafkaConsumer` creates a [Fetcher](Fetcher.md) when [created](#creating-instance).

## <span id="coordinator"> ConsumerCoordinator

`KafkaConsumer` creates a [ConsumerCoordinator](ConsumerCoordinator.md) when [created](#creating-instance) with the [group.id](#groupId) specified.

## <span id="enforceRebalance"> enforceRebalance

```java
void enforceRebalance()
```

`enforceRebalance` requests the [ConsumerCoordinator](#coordinator) to [requestRejoin](ConsumerCoordinator.md#requestRejoin) with the following reason:

```text
rebalance enforced by user
```

`enforceRebalance` is part of the [Consumer](Consumer.md#enforceRebalance) abstraction.

## <span id="groupMetadata"> groupMetadata

```java
ConsumerGroupMetadata groupMetadata()
```

`groupMetadata`...FIXME

`groupMetadata` is part of the [Consumer](Consumer.md#groupMetadata) abstraction.

## <span id="poll"> Polling for Records

```java
ConsumerRecords<K, V> poll(
  Duration timeout) // (1)
ConsumerRecords<K, V> poll(
  Timer timer,
  boolean includeMetadataInTimeout) // (2)
```

1. Uses `includeMetadataInTimeout` enabled (`true`)
2. A private method

`poll`...FIXME

`poll` is part of the [Consumer](Consumer.md#poll) abstraction.

### <span id="pollForFetches"> pollForFetches

```java
Map<TopicPartition, List<ConsumerRecord<K, V>>> pollForFetches(
  Timer timer)
```

`pollForFetches` requests the [Fetcher](#fetcher) for [fetchedRecords](Fetcher.md#fetchedRecords) and returns them immediately if available.

Otherwise, `pollForFetches` requests the [Fetcher](#fetcher) to [sendFetches](Fetcher.md#sendFetches).

`pollForFetches`...FIXME

## <span id="subscribe"> Subscribing to Topics

```java
void subscribe(
  Collection<String> topics) // (1)
void subscribe(
  Collection<String> topics,
  ConsumerRebalanceListener listener)
void subscribe(
  Pattern pattern) // (2)
void subscribe(
  Pattern pattern,
  ConsumerRebalanceListener callback)
```

1. Uses `NoOpConsumerRebalanceListener`
1. Uses `NoOpConsumerRebalanceListener`

`subscribe`...FIXME

`subscribe` is part of the [Consumer](Consumer.md#subscribe) abstraction.

## <span id="wakeup"> Waking Up

```java
void wakeup()
```

`wakeup`...FIXME

`wakeup` is part of the [Consumer](Consumer.md#wakeup) abstraction.
