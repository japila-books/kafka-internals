# ConsumerPartitionAssignor

`ConsumerPartitionAssignor` is an [abstraction](#contract) of [partition assignors](#implementations).

## Contract

### <span id="assign"> assign

```java
GroupAssignment assign(
  Cluster metadata,
  GroupSubscription groupSubscription)
```

Used when:

* FIXME

### <span id="name"> name

```java
String name()
```

Used when:

* FIXME

## Implementations

* [AbstractPartitionAssignor](AbstractPartitionAssignor.md)
* `StreamsPartitionAssignor` ([Kafka Streams]({{ book.kafka_streams }}/StreamsPartitionAssignor))

## <span id="onAssignment"> onAssignment

```java
void onAssignment(
  Assignment assignment,
  ConsumerGroupMetadata metadata)
```

`onAssignment`...FIXME

`onAssignment` is used when:

* FIXME

## <span id="supportedProtocols"> supportedProtocols

```java
List<RebalanceProtocol> supportedProtocols()
```

Default: `RebalanceProtocol.EAGER`

`supportedProtocols` is used when:

* FIXME

## <span id="subscriptionUserData"> subscriptionUserData

```java
ByteBuffer subscriptionUserData(
  Set<String> topics)
```

`subscriptionUserData` is `null` by default.

`subscriptionUserData` is used when:

* `ConsumerCoordinator` is requested for [metadata](ConsumerCoordinator.md#metadata)
