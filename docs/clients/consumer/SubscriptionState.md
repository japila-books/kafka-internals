# SubscriptionState

## Preferred Read Replica

### <span id="preferredReadReplica"> preferredReadReplica

```java
Optional<Integer> preferredReadReplica(
  TopicPartition tp,
  long timeMs)
```

`preferredReadReplica` [looks up the state](#assignedStateOrNull) of the given `TopicPartition` and, if found, requests it for the [preferredReadReplica](TopicPartitionState.md#preferredReadReplica). Otherwise, `preferredReadReplica` returns an undefined preferred read replica.

`preferredReadReplica` is used when:

* `Fetcher` is requested to [selectReadReplica](Fetcher.md#selectReadReplica)

### <span id="updatePreferredReadReplica"> updatePreferredReadReplica

```java
void updatePreferredReadReplica(
  TopicPartition tp,
  int preferredReadReplicaId,
  LongSupplier timeMs)
```

`updatePreferredReadReplica` [looks up the state](#assignedState) of the given `TopicPartition` and requests it to [updatePreferredReadReplica](TopicPartitionState.md#updatePreferredReadReplica).

`updatePreferredReadReplica` is used when:

* `Fetcher` is requested to [initializeCompletedFetch](Fetcher.md#initializeCompletedFetch)

### <span id="clearPreferredReadReplica"> clearPreferredReadReplica

```java
Optional<Integer> clearPreferredReadReplica(
  TopicPartition tp)
```

`clearPreferredReadReplica` [looks up the state](#assignedState) of the given `TopicPartition` and requests it to [clearPreferredReadReplica](TopicPartitionState.md#clearPreferredReadReplica).

`clearPreferredReadReplica` is used when:

* `Fetcher` is requested to [selectReadReplica](Fetcher.md#selectReadReplica) and [initializeCompletedFetch](Fetcher.md#initializeCompletedFetch)
