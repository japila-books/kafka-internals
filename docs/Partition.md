# Partition

## <span id="readRecords"> readRecords

```scala
readRecords(
  lastFetchedEpoch: Optional[Integer],
  fetchOffset: Long,
  currentLeaderEpoch: Optional[Integer],
  maxBytes: Int,
  fetchIsolation: FetchIsolation,
  fetchOnlyFromLeader: Boolean,
  minOneMessage: Boolean): LogReadInfo
```

`readRecords`...FIXME

In the end, `readRecords` requests the `Log` to [read messages](Log.md#read).

`readRecords` is used when:

* `ReplicaManager` is requested to [readFromLocalLog](ReplicaManager.md#readFromLocalLog)
