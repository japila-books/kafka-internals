# Demo: Using kafka-leader-election for Partition Leader Election

## Review Me

The demo shows how to use link:kafka-tools-kafka-leader-election.adoc[kafka-leader-election.sh] command-line utility for link:kafka-partition-leader-election.adoc[Partition Leader Election].

The demo is made up of the following steps:

. <<step-1, Start Kafka Cluster>>

. <<step-2, Create Topic>>

. <<step-3, Shutting Down Leader Broker>>

. <<step-4, Trigger Preferred Leader Election>>

=== [[step-1]] Start Kafka Cluster

You'll be using https://github.com/wurstmeister/kafka-docker[kafka-docker] project to run Apache Kafka in Docker (using Docker Compose).

```
docker-compose up -d --scale=kafka=3
```

The goal is to run 3 brokers as easily as possible (_let me know if you know a simpler way. Thanks!_).

=== [[step-2]] Create Topic

In this step you'll create a topic with 3 partitions (one for every broker in the cluster) and 2 replicas.

Log in to `kafka-docker_kafka_1` broker first.

```
docker exec -it kafka-docker_kafka_1 /bin/bash
```

```
kafka-topics.sh \
    --bootstrap-server :9092 \
    --create \
    --topic demo-kafka-leader-election \
    --partitions 3 \
    --replication-factor 2
```

Review the leader and ISR brokers. Yours may be different.

```
$ kafka-topics.sh \
    --bootstrap-server :9092 \
    --describe \
    --topic demo-kafka-leader-election
Topic: demo-kafka-leader-election	PartitionCount: 3	ReplicationFactor: 2	Configs: segment.bytes=1073741824
	Topic: demo-kafka-leader-election	Partition: 0	Leader: 1002	Replicas: 1002,1001	Isr: 1002,1001
	Topic: demo-kafka-leader-election	Partition: 1	Leader: 1001	Replicas: 1001,1003	Isr: 1001,1003
	Topic: demo-kafka-leader-election	Partition: 2	Leader: 1003	Replicas: 1003,1002	Isr: 1003,1002
```

=== [[step-3]] Shutting Down Leader Broker

In this step, you're going to shut down a leader broker to simulate a broker failure. That triggers a leader election and promote the other broker (in the ISR list) to become the leader.

```
docker stop kafka-docker_kafka_2
```

You should have 2 brokers up and running.

```
$ docker ps
CONTAINER ID        IMAGE                    COMMAND                  CREATED             STATUS              PORTS                                                NAMES
f7224b2bdf90        kafka-docker_kafka       "start-kafka.sh"         2 minutes ago       Up 2 minutes        0.0.0.0:32785->9092/tcp                              kafka-docker_kafka_3
52ac4b64648e        wurstmeister/zookeeper   "/bin/sh -c '/usr/sb…"   2 minutes ago       Up 2 minutes        22/tcp, 2888/tcp, 3888/tcp, 0.0.0.0:2181->2181/tcp   kafka-docker_zookeeper_1
ad05db816dc1        kafka-docker_kafka       "start-kafka.sh"         2 minutes ago       Up 2 minutes        0.0.0.0:32784->9092/tcp                              kafka-docker_kafka_1
```

Review the leaders.

```
$ kafka-topics.sh \
    --bootstrap-server :9092 \
    --describe \
    --topic demo-kafka-leader-election
Topic: demo-kafka-leader-election	PartitionCount: 3	ReplicationFactor: 2	Configs: segment.bytes=1073741824
	Topic: demo-kafka-leader-election	Partition: 0	Leader: 1001	Replicas: 1002,1001	Isr: 1001
	Topic: demo-kafka-leader-election	Partition: 1	Leader: 1001	Replicas: 1001,1003	Isr: 1001,1003
	Topic: demo-kafka-leader-election	Partition: 2	Leader: 1003	Replicas: 1003,1002	Isr: 1003
```

=== [[step-4]] Trigger Preferred Leader Election

In this step, you will trigger link:kafka-tools-kafka-leader-election.adoc#preferred-partition-leader-election[preferred leader election] for the partitions with the other leader elected (_non-preferred leader_).

Let's bring the stopped broker up.

```
$ docker start kafka-docker_kafka_2
kafka-docker_kafka_2
```

Review the leaders.

```
$ kafka-topics.sh \
    --bootstrap-server :9092 \
    --describe \
    --topic demo-kafka-leader-election
Topic: demo-kafka-leader-election	PartitionCount: 3	ReplicationFactor: 2	Configs: segment.bytes=1073741824
	Topic: demo-kafka-leader-election	Partition: 0	Leader: 1001	Replicas: 1002,1001	Isr: 1001,1002
	Topic: demo-kafka-leader-election	Partition: 1	Leader: 1001	Replicas: 1001,1003	Isr: 1001,1003
	Topic: demo-kafka-leader-election	Partition: 2	Leader: 1003	Replicas: 1003,1002	Isr: 1003,1002
```

Time to fix the partition `0` so the broker `1002` is the leader again.

```
kafka-leader-election.sh \
  --bootstrap-server :9092 \
  --topic demo-kafka-leader-election \
  --partition 0 \
  --election-type preferred
```

You should see the following message for a successful election.

```
Successfully completed leader election (PREFERRED) for partitions demo-kafka-leader-election-0
```

Review the leaders.

```
$ kafka-topics.sh \
    --bootstrap-server :9092 \
    --describe \
    --topic demo-kafka-leader-election
Topic: demo-kafka-leader-election	PartitionCount: 3	ReplicationFactor: 2	Configs: segment.bytes=1073741824
	Topic: demo-kafka-leader-election	Partition: 0	Leader: 1002	Replicas: 1002,1001	Isr: 1001,1002
	Topic: demo-kafka-leader-election	Partition: 1	Leader: 1001	Replicas: 1001,1003	Isr: 1001,1003
	Topic: demo-kafka-leader-election	Partition: 2	Leader: 1003	Replicas: 1003,1002	Isr: 1003,1002
```
