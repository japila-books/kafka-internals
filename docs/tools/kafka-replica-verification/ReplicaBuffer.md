# ReplicaBuffer

## Creating Instance

`ReplicaBuffer` takes the following to be created:

* <span id="expectedReplicasPerTopicPartition"> Expected Replicas per TopicPartition (`Map[TopicPartition, Int]`)
* <span id="initialOffsets"> Initial offsets (`Map[TopicPartition, Long]`)
* <span id="expectedNumFetchers"> Expected number of fetchers
* [Report interval](#reportInterval)

`ReplicaBuffer` is created when:

* `ReplicaVerificationTool` is [executed](ReplicaVerificationTool.md#main)

## <span id="reportInterval"> Report Interval

`ReplicaBuffer` is given a report interval when [created](#creating-instance).

The value is `--report-interval-ms` and defaults to `30s`.

The interval is used when [verifyCheckSum](#verifyCheckSum) to print out the following to the standard output:

```text
[currentTimeMs]: max lag is [lag] for partition [partition] at offset [offset] among [n] partitions
```
