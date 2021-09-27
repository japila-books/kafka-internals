# TopicPartitionState

# <span id="preferredReadReplica"><span id="updatePreferredReadReplica"><span id="clearPreferredReadReplica"> preferredReadReplica

```java
Integer preferredReadReplica
```

`TopicPartitionState` manages the preferred read replica (of a `TopicPartition`) for a specified amount of time (until expires or is cleared out).

`preferredReadReplica` is used when:

* `SubscriptionState` is requested for the [preferredReadReplica](SubscriptionState.md#preferredReadReplica), [updatePreferredReadReplica](SubscriptionState.md#updatePreferredReadReplica) and [clearPreferredReadReplica](SubscriptionState.md#clearPreferredReadReplica)
