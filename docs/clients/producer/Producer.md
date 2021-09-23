# Producer

`Producer<K, V>` is an [interface](#contract) to [KafkaProducer](KafkaProducer.md) for Kafka developers to use to send records (with `K` keys and `V` values) to a Kafka cluster.

## Contract (Subset)

### <span id="abortTransaction"> abortTransaction

```java
void abortTransaction()
```

### <span id="beginTransaction"> beginTransaction

```java
void beginTransaction()
```

### <span id="commitTransaction"> commitTransaction

```java
void commitTransaction()
```

### <span id="initTransactions"> initTransactions

```java
void initTransactions()
```

### <span id="sendOffsetsToTransaction"> sendOffsetsToTransaction

```java
void sendOffsetsToTransaction(
  Map<TopicPartition, OffsetAndMetadata> offsets,
  ConsumerGroupMetadata groupMetadata)
```

Used when the producer is also a [Consumer](../consumer/index.md) for a consume-transform-produce pattern
