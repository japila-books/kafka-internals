# Partitioner

`Partitioner` is an [abstraction](#contract) of [partitioners](#implementations) for a [KafkaProducer](KafkaProducer.md#partitioner) to determine the [partition](#partition) of records (to be [sent out](KafkaProducer.md#send)).

## <span id="Configurable"> Configurable

`Partitioner` is a [Configurable](../Configurable.md).

## <span id="Closeable"> Closeable

`Partitioner` is a `Closeable` ([Java]({{ java.api }}/java/io/Closeable.html)).

## Contract

### <span id="onNewBatch"> onNewBatch

```java
void onNewBatch(
  String topic,
  Cluster cluster,
  int prevPartition)
```

Used when:

* `KafkaProducer` is requested to [send a record](KafkaProducer.md#send) (and [doSend](KafkaProducer.md#doSend))

### <span id="partition"> Computing Partition

```java
int partition(
  String topic,
  Object key,
  byte[] keyBytes,
  Object value,
  byte[] valueBytes,
  Cluster cluster)
```

Used when:

* `KafkaProducer` is requested to [send a record](KafkaProducer.md#send) (and determines the [partition](KafkaProducer.md#partition))

## Implementations

* [DefaultPartitioner](DefaultPartitioner.md)
* UniformStickyPartitioner
* RoundRobinPartitioner
