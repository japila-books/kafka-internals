# SubscriptionState

## <span id="updatePreferredReadReplica"> updatePreferredReadReplica

```java
void updatePreferredReadReplica(
  TopicPartition tp,
  int preferredReadReplicaId,
  LongSupplier timeMs)
```

`updatePreferredReadReplica`...FIXME

`updatePreferredReadReplica` is used when:

* `Fetcher` is requested to [initializeCompletedFetch](Fetcher.md#initializeCompletedFetch)

## <span id="preferredReadReplica"> preferredReadReplica

```java
Optional<Integer> preferredReadReplica(
  TopicPartition tp,
  long timeMs)
```

`preferredReadReplica`...FIXME

`preferredReadReplica` is used when:

* `Fetcher` is requested to [selectReadReplica](Fetcher.md#selectReadReplica)
