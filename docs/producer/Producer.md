# Producer

`Producer<K, V>` is an [interface](#contract) to [KafkaProducer](KafkaProducer.md) for Kafka developers to use to send messages (with `K` keys and `V` values) to a Kafka cluster.

## Contract (Subset)

### <span id="abortTransaction"> abortTransaction

```java
void abortTransaction()
```

### <span id="beginTransaction"> beginTransaction

```java
void beginTransaction()
```

Used when:

* FIXME

### <span id="commitTransaction"> commitTransaction

```java
void commitTransaction()
```

Used when:

* FIXME

### <span id="initTransactions"> initTransactions

```java
void initTransactions()
```

Used when:

* FIXME

### <span id="sendOffsetsToTransaction"> sendOffsetsToTransaction

```java
void sendOffsetsToTransaction(
  Map<TopicPartition, OffsetAndMetadata> offsets,
  String consumerGroupId)
void sendOffsetsToTransaction(
  Map<TopicPartition, OffsetAndMetadata> offsets,
  ConsumerGroupMetadata groupMetadata)
```

Used when:

* FIXME
