# Partition Leader Election

**Partition Leader Election** is a process of electing a broker as the leader of a partition.

Use [kafka-leader-election](../tools/kafka-leader-election/index.md) utility for [preferred](#preferred-partition-leader-election) or [unclean](#unclean-partition-leader-election) leader election.

!!! note
    `kafka-preferred-replica-election.sh` tool has been deprecated since Kafka 2.4.0 (cf. [KIP-460: Admin Leader Election RPC](https://cwiki.apache.org/confluence/display/KAFKA/KIP-460%3A+Admin+Leader+Election+RPC)).

Observe `state.change.logger` (default: `state-change.log`) to trace the process in the logs.

Internally, Kafka controller uses [Election](../controller/Election.md) utility (and [PartitionLeaderElectionAlgorithms](../controller/PartitionLeaderElectionAlgorithms.md)) for the algorithms for partition leader election.

## Preferred Partition Leader Election

**Preferred Partition Leader Election** is...FIXME

## Unclean Partition Leader Election

**Unclean Partition Leader Election** allows a [non-ISR replica broker to be elected as a partition leader](../controller/PartitionLeaderElectionAlgorithms.md#offlinePartitionLeaderElection) (as the last resort since doing so may result in data loss).

[unclean.leader.election.enable](../KafkaConfig.md#unclean.leader.election.enable) configuration property is used to enable it cluster-wide (for any topic) or per topic.

Enable INFO logging level for [kafka.controller.KafkaController](../controller/KafkaController.md#logging) logger to observe the [process](../controller/KafkaController.md#processUncleanLeaderElectionEnable) in the logs.

## Demo

[Demo: Using kafka-leader-election](../demo/partition-leader-election.md).
