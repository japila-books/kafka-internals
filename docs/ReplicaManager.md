# ReplicaManager

## <span id="fetchMessages"> fetchMessages

```scala
fetchMessages(
  timeout: Long,
  replicaId: Int,
  fetchMinBytes: Int,
  fetchMaxBytes: Int,
  hardMaxBytesLimit: Boolean,
  fetchInfos: Seq[(TopicPartition, PartitionData)],
  quota: ReplicaQuota,
  responseCallback: Seq[(TopicPartition, FetchPartitionData)] => Unit,
  isolationLevel: IsolationLevel,
  clientMetadata: Option[ClientMetadata]): Unit
```

`fetchMessages` determines whether the request comes from a follower or a consumer (based on the given `replicaId`).

`fetchMessages` determines `FetchIsolation`:

* `FetchLogEnd` if the request comes from a follower
* `FetchTxnCommitted` if the request comes from a consumer with `READ_COMMITTED` isolation level
* `FetchHighWatermark` otherwise

`fetchMessages` [readFromLocalLog](#readFromLocalLog) (passing in the `FetchIsolation` among the others).

`fetchMessages`...FIXME

`fetchMessages` is used when:

* `KafkaApis` is requested to [handle a Fetch request](KafkaApis.md#handleFetchRequest)
* `ReplicaAlterLogDirsThread` is requested to [fetchFromLeader](ReplicaAlterLogDirsThread.md#fetchFromLeader)

## <span id="readFromLocalLog"> readFromLocalLog

```scala
readFromLocalLog(
  replicaId: Int,
  fetchOnlyFromLeader: Boolean,
  fetchIsolation: FetchIsolation,
  fetchMaxBytes: Int,
  hardMaxBytesLimit: Boolean,
  readPartitionInfo: Seq[(TopicPartition, PartitionData)],
  quota: ReplicaQuota,
  clientMetadata: Option[ClientMetadata]): Seq[(TopicPartition, LogReadResult)]
```

`readFromLocalLog`...FIXME

`readFromLocalLog` finds the `Partition` and requests it to [readRecords](Partition.md#readRecords).

`readFromLocalLog`...FIXME

`readFromLocalLog` is used when:

* `DelayedFetch` is requested to `onComplete`
* `ReplicaManager` is requested to [fetchMessages](#fetchMessages)

## <span id="becomeLeaderOrFollower"> becomeLeaderOrFollower

```scala
becomeLeaderOrFollower(
  correlationId: Int,
  leaderAndIsrRequest: LeaderAndIsrRequest,
  onLeadershipChange: (Iterable[Partition], Iterable[Partition]) => Unit): LeaderAndIsrResponse
```

`becomeLeaderOrFollower`...FIXME

---

`becomeLeaderOrFollower` is used when:

* `KafkaApis` is requested to [handleLeaderAndIsrRequest](KafkaApis.md#handleLeaderAndIsrRequest)

### <span id="makeLeaders"> makeLeaders

```scala
makeLeaders(
  controllerId: Int,
  controllerEpoch: Int,
  partitionStates: Map[Partition, LeaderAndIsrPartitionState],
  correlationId: Int,
  responseMap: mutable.Map[TopicPartition, Errors],
  highWatermarkCheckpoints: OffsetCheckpoints,
  topicIds: String => Option[Uuid]): Set[Partition]
```

`makeLeaders`...FIXME
