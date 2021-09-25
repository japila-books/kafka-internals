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

`fetchMessages`...FIXME

`fetchMessages` is used when:

* `KafkaApis` is requested to [handle a Fetch request](KafkaApis.md#handleFetchRequest)
* `ReplicaAlterLogDirsThread` is requested to [fetchFromLeader](ReplicaAlterLogDirsThread.md#fetchFromLeader)
