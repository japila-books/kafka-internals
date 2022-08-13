# Kafka Raft (KRaft)

[KIP-500](https://cwiki.apache.org/confluence/display/KAFKA/KIP-500%3A+Replace+ZooKeeper+with+a+Self-Managed+Metadata+Quorum) allows running a Kafka cluster without Apache ZooKeeper in so-called **Kafka Raft metadata mode** (_KRaft mode_).

!!! important "Preview and Not for Production Deployments"
    KRaft mode is currently _PREVIEW AND SHOULD NOT BE USED IN PRODUCTION_.

## Raft-Based Metadata Quorum
