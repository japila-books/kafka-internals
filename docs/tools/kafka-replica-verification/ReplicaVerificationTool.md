# ReplicaVerificationTool

`ReplicaVerificationTool` is a [command-line application](#main) for [kafka-replica-verification](index.md) to perform [replica verification](ReplicaFetcher.md#doWork-verification).

## <span id="main"> Executing Application

`main` prints out the following INFO message to the logs:

```text
Getting topic metadata...
```

`main` [createAdminClient](#createAdminClient) (with the brokers in `--broker-list` option) for [topics metadata](#listTopicsMetadata) and [broker info](#brokerDetails).

`main` creates `TopicPartitionReplica`s for the topics.

`main` prints out the following DEBUG message to the logs:

```text
Selected topic partitions: [topicPartitionReplicas]
```

`main` groups partitions per broker and prints out the following DEBUG message to the logs:

```text
Topic partitions per broker: [brokerToTopicPartitions]
```

`main` groups partitions per replica to count the number of replicas and prints out the following DEBUG message to the logs:

```text
Expected replicas per topic partition: [expectedReplicasPerTopicPartition]
```

`main` [creates a consumer config](#consumerConfig).

`main` creates [ReplicaFetcher](ReplicaFetcher.md)s for every replica broker and [starts them all](ReplicaFetcher.md#doWork).

`ReplicaFetcher`s run until termination (Ctrl-C). `main` prints out the following INFO message to the logs:

```text
Stopping all fetchers
```
