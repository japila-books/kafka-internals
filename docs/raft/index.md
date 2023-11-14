# Kafka Raft (KRaft)

**Kafka Raft (KRaft)** is a new [Raft](https://en.wikipedia.org/wiki/Raft_(algorithm))-based distributed consensus algorithm for metadata storage (maintenance).

KRaft allows running a Kafka cluster without Apache ZooKeeper in so-called **Kafka Raft metadata mode** (_KRaft mode_).

With KRaft, a Kafka cluster uses its own Kafka infrastructure for metadata (with no need for a separate system, e.g. Zookeeper).

In KRaft, a server can be a `broker`, [controller](#controllers) or both (`broker,controller`) based on [process.roles](../KafkaConfig.md#process.roles) configuration property.

KRaft was proposed as [KIP-500]({{ kafka.wiki }}/KIP-500%3A+Replace+ZooKeeper+with+a+Self-Managed+Metadata+Quorum) and went live in [Kafka 2.8.0]({{ kafka.jira }}/KAFKA-9119).

KRaft is production ready since [Apache Kafka 3.3.1]({{ kafka.wiki }}/KIP-833%3A+Mark+KRaft+as+Production+Ready).

## Controllers

KRaft Controllers are responsible for storing the metadata of the cluster in the metadata log.

Controllers participate in the [metadata quorum](#metadata-quorum).

## Raft-Based Metadata Quorum { #metadata-quorum }

## KRaft Metadata Transactions

[KIP-868 Metadata Transactions]({{ kafka.wiki }}/KIP-868+Metadata+Transactions)

## Learn More

* [Raft (algorithm)](https://en.wikipedia.org/wiki/Raft_(algorithm)) on Wikipedia
* [KIP-500]({{ kafka.wiki }}/KIP-500%3A+Replace+ZooKeeper+with+a+Self-Managed+Metadata+Quorum)
* [KRaft Overview](https://docs.confluent.io/platform/current/kafka-metadata/kraft.html)
