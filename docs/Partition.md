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

---

`readRecords` is used when:

* `ReplicaManager` is requested to [readFromLocalLog](ReplicaManager.md#readFromLocalLog)

## <span id="makeLeader"> makeLeader

```scala
makeLeader(
  partitionState: LeaderAndIsrPartitionState,
  highWatermarkCheckpoints: OffsetCheckpoints,
  topicId: Option[Uuid]): Boolean
```

`makeLeader`...FIXME

---

`makeLeader` is used when:

* `ReplicaManager` is requested to [makeLeaders](ReplicaManager.md#makeLeaders), [applyLocalLeadersDelta](ReplicaManager.md#applyLocalLeadersDelta)

## <span id="createLogIfNotExists"> createLogIfNotExists

```scala
createLogIfNotExists(
  isNew: Boolean,
  isFutureReplica: Boolean,
  offsetCheckpoints: OffsetCheckpoints,
  topicId: Option[Uuid]): Unit
```

`createLogIfNotExists`...FIXME

---

`createLogIfNotExists` is used when:

* `Partition` is requested to [maybeCreateFutureReplica](#maybeCreateFutureReplica), [makeLeader](#makeLeader), [makeFollower](#makeFollower)
* `ReplicaManager` is requested to [maybeAddLogDirFetchers](ReplicaManager.md#maybeAddLogDirFetchers), [makeFollowers](ReplicaManager.md#makeFollowers), [applyLocalFollowersDelta](ReplicaManager.md#applyLocalFollowersDelta)

### <span id="createLog"> createLog

```scala
createLog(
  isNew: Boolean,
  isFutureReplica: Boolean,
  offsetCheckpoints: OffsetCheckpoints,
  topicId: Option[Uuid]): UnifiedLog
```

`createLog`...FIXME
