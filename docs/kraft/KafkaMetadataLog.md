# KafkaMetadataLog

`KafkaMetadataLog` is a `ReplicatedLog`.

## Creating Instance

`KafkaMetadataLog` takes the following to be created:

* <span id="log"> [UnifiedLog](../log/UnifiedLog.md)
* <span id="time"> `Time`
* <span id="scheduler"> `Scheduler`
* <span id="snapshots"> Snapshots
* <span id="topicPartition"> `TopicPartition`
* <span id="config"> `MetadataLogConfig`

`KafkaMetadataLog` is created using [apply](#apply) utility.

## Creating KafkaMetadataLog { #apply }

```scala
apply(
  topicPartition: TopicPartition,
  topicId: Uuid,
  dataDir: File,
  time: Time,
  scheduler: Scheduler,
  config: MetadataLogConfig): KafkaMetadataLog
```

`apply`...FIXME

---

`apply` is used when:

* `KafkaRaftManager` is requested to [buildMetadataLog](KafkaRaftManager.md#buildMetadataLog)
