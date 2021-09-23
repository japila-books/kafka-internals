# Consumer

`Consumer<K, V>` is an [interface](#contract) to [KafkaConsumer](KafkaConsumer.md) for Kafka developers to use to consume records (with `K` keys and `V` values) from a Kafka cluster.

## Contract (Subset)

### <span id="enforceRebalance"> enforceRebalance

```java
void enforceRebalance()
```

### <span id="groupMetadata"> groupMetadata

```java
ConsumerGroupMetadata groupMetadata()
```

### <span id="subscribe"> Subscribing to Topics

```java
void subscribe(
  Collection<String> topics)
void subscribe(
  Collection<String> topics,
  ConsumerRebalanceListener callback)
void subscribe(
  Pattern pattern)
void subscribe(
  Pattern pattern,
  ConsumerRebalanceListener callback)
```

### <span id="wakeup"> Waking Up

```java
void wakeup()
```
