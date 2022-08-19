# ReassignPartitionsCommand

## Review Me

`kafka.admin.ReassignPartitionsCommand` is a <<main, command-line tool>> that allows for <<generate, generating>>, <<execute, executing>> and <<verify, verifying>> a custom partition (re)assignment configuration (as specified using a <<reassignment-json-file, reassignment JSON file>>).

[[actions]]
.ReassignPartitionsCommand's Actions
[cols="1,2",options="header",width="100%"]
|===
| Action
| Description

| <<executeAssignment, --execute>>
a| [[execute]] Executes the reassignment as specified by the <<reassignment-json-file, reassignment-json-file>> option

| <<generateAssignment, --generate>>
a| [[generate]] Generates a candidate partition reassignment configuration

NOTE: This only generates a candidate assignment, it does not execute it.

| <<verifyAssignment, --verify>>
a| [[verify]] Verifies if the reassignment completed as specified by the <<reassignment-json-file, reassignment-json-file>> option. If there is a throttle engaged for the replicas specified, and the rebalance has completed, the throttle will be removed
|===

`ReassignPartitionsCommand` can be executed using `kafka-reassign-partitions` shell script (i.e. `bin/kafka-reassign-partitions.sh` or `bin\windows\kafka-reassign-partitions.bat`).

```
$ ./bin/kafka-reassign-partitions.sh
This command moves topic partitions between replicas.
```

[[options]]
.ReassignPartitionsCommand's Options
[cols="1m,2",options="header",width="100%"]
|===
| Option
| Description

| reassignment-json-file
a| [[reassignment-json-file]] A JSON file with a custom partition (re)assignment configuration

The format to use is as follows:

```
{
  "partitions": [
    {
      "topic": "foo",
      "partition": 1,
      "replicas": [
        1,
        2,
        3
      ],
      "log_dirs": [
        "dir1",
        "dir2",
        "dir3"
      ]
    }
  ],
  "version": 1
}
```

Note that "log_dirs" is optional. When specified, its length must equal the length of the replicas list. The value in this list can be either `"any"` or the absolute path of the log directory on the broker.

If absolute log directory path is specified, it is currently required that the replica has not already been created on that broker. The replica will then be created in the specified log directory on the broker later.

| replica-alter-log-dirs-throttle
a| [[replica-alter-log-dirs-throttle]] The movement of replicas between log directories on the same broker will be throttled to this value (bytes/sec).

Default: `-1`

Rerunning with this option, whilst a rebalance is in progress, will alter the throttle value. The throttle rate should be at least 1 KB/s.

| throttle
a| [[throttle]] The movement of partitions between brokers will be throttled to this value (bytes/sec).

Default: `-1`

Rerunning with this option, whilst a rebalance is in progress, will alter the throttle value. The throttle rate should be at least 1 KB/s.

| timeout
a| [[timeout]] The maximum time (in ms) allowed to wait for partition reassignment execution to be successfully initiated

Default: `10000`
|===

```
$ ./bin/kafka-topics.sh --list --zookeeper :2181
my-topic

$ cat reassign-partitions.json
{
  "partitions": [
    {
      "topic": "my-topic",
      "partition": 1,
      "replicas": [
        1
      ]
    }
  ],
  "version": 1
}

$ ./bin/kafka-reassign-partitions.sh \
  --generate \
  --zookeeper :2181 \
  --topics-to-move-json-file reassign-partitions.json \
  --broker-list 0

$ ./bin/kafka-reassign-partitions.sh \
  --verify \
  --zookeeper :2181 \
  --reassignment-json-file reassign-partitions.json
```

`ReassignPartitionsCommand` is <<creating-instance, created>> exclusively when `ReassignPartitionsCommand` is requested to <<executeAssignment, executeAssignment>>.

=== [[main]] Executing Standalone Application -- `main` Method

[source, scala]
----
main(args: Array[String]): Unit
----

`main`...FIXME

=== [[existingAssignment]] `existingAssignment` Method

[source, scala]
----
existingAssignment(): Map[TopicPartition, Seq[Int]]
----

`existingAssignment` takes the topics (from the keys) from the <<proposedPartitionAssignment, proposedPartitionAssignment>> and requests the <<zkClient, KafkaZkClient>> to <<kafka-zk-KafkaZkClient.adoc#getReplicaAssignmentForTopics, getReplicaAssignmentForTopics>>.

NOTE: `existingAssignment` is used when...FIXME

=== [[creating-instance]] Creating ReassignPartitionsCommand Instance

`ReassignPartitionsCommand` takes the following when created:

* [[zkClient]] <<kafka-zk-KafkaZkClient.adoc#, KafkaZkClient>>
* [[adminClientOpt]] Optional <<kafka-clients-admin-AdminClient.adoc#, AdminClient>>
* [[proposedPartitionAssignment]] Proposed partition assignment (`Map[TopicPartition, Seq[Int]]`)
* [[proposedReplicaAssignment]] Proposed replica assignment (`Map[TopicPartitionReplica, String]`)
* [[adminZkClient]] <<kafka-zk-AdminZkClient.adoc#, AdminZkClient>>
