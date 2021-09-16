# {{ book.title }}

[Apache Kafka](https://kafka.apache.org/) is an open source project for a distributed publish-subscribe messaging system rethought as a **distributed commit log**.

## Messages

Messages (_records_, _events_) are byte arrays (String, JSON, and Avro are among the most common formats).
If a message has a key, Kafka (uses [Partitioner](clients/producer/Partitioner.md)) to make sure that all messages of the same key are in the same partition.

## Topics

Kafka stores messages in topics that are partitioned and replicated across multiple brokers in a cluster.

## Kafka Clients

[Producers](clients/producer/index.md) send messages to topics from which [consumers](clients/consumer/index.md) read.

## Language Agnostic

Kafka clients use binary protocol to talk to a Kafka cluster.

## Consumer Groups

Consumers may be grouped in a consumer group with multiple consumers. Each consumer in a consumer group will read messages from a unique subset of partitions in each topic they subscribe to. Each message is delivered to one consumer in the group, and all messages with the same key arrive to the same consumer.

## Durability

Kafka does not track which messages were read by consumers. Kafka keeps all messages for a finite amount of time, and it is consumers' responsibility to track their location per topic (offsets).
