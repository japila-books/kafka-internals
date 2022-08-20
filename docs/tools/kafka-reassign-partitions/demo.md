# Demo: kafka-reassign-partitions

## Set Up Kafka Cluster

Start a 3-broker Kafka cluster.

```shell
./bin/kafka-server-start.sh config/server.properties \
  --override broker.id=10 \
  --override log.dirs=/tmp/kafka-logs-10 \
  --override listeners=PLAINTEXT://:9192
```

```shell
./bin/kafka-server-start.sh config/server.properties \
  --override broker.id=20 \
  --override log.dirs=/tmp/kafka-logs-20 \
  --override listeners=PLAINTEXT://:9292
```

```shell
./bin/kafka-server-start.sh config/server.properties \
  --override broker.id=30 \
  --override log.dirs=/tmp/kafka-logs-30 \
  --override listeners=PLAINTEXT://:9392
```

## Create Topic

```shell
./bin/kafka-topics.sh \
  --bootstrap-server :9192 \
  --create \
  --topic demo-reassign-partitions \
  --replication-factor 2 \
  --partitions 1
```

```shell
./bin/kafka-topics.sh \
  --bootstrap-server :9192 \
  --describe \
  --topic demo-reassign-partitions
```

```text
Topic: demo-reassign-partitions TopicId: J3ginqnTTg-LjVoI20dqqw PartitionCount: 1 ReplicationFactor: 2 Configs: segment.bytes=1073741824
 Topic: demo-reassign-partitions Partition: 0 Leader: 10 Replicas: 10,20 Isr: 10,20
```

## reassign-partitions.json

```json title="reassign-partitions.json"
{
  "partitions": [
    {
      "topic": "demo-reassign-partitions",
      "partition": 1,
      "replicas": [
        20,
        30
      ]
    }
  ],
  "version": 1
}
```

## Generate Reassignment Configuration

!!! FIXME "Does not seem to work"

```console
$ ./bin/kafka-reassign-partitions.sh \
  --bootstrap-server :9192 \
  --generate \
  --topics-to-move-json-file reassign-partitions.json \
  --broker-list 10,20,30
```

## Verify Reassignment Configuration

``` console
$ ./bin/kafka-reassign-partitions.sh \
  --bootstrap-server :9192 \
  --verify \
  --reassignment-json-file reassign-partitions.json
```
