# Kafka Raft (KRaft)

**KRaft** is a new consensus algorithm that allows running a Kafka cluster without Apache ZooKeeper in so-called **Kafka Raft metadata mode** (_KRaft mode_).

KRaft was proposed as [KIP-500](https://cwiki.apache.org/confluence/display/KAFKA/KIP-500%3A+Replace+ZooKeeper+with+a+Self-Managed+Metadata+Quorum) and went live in [Kafka 2.8.0]({{ kafka.jira }}/KAFKA-9119).

KRaft is production ready since [Apache Kafka 3.3.1]({{ kafka.wiki }}/KIP-833%3A+Mark+KRaft+as+Production+Ready).

## Raft-Based Metadata Quorum

## KRaft Metadata Transactions

[KIP-868 Metadata Transactions]({{ kafka.wiki }}/KIP-868+Metadata+Transactions)
