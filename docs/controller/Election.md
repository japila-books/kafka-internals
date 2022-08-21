# Election

`Election` is a utility with the algorithms for [partition leader election](../partition-leader-election/index.md).

## <span id="leaderForOffline"> leaderForOffline

```scala
leaderForOffline(
  controllerContext: ControllerContext,
  isLeaderRecoverySupported: Boolean,
  partitionsWithUncleanLeaderRecoveryState: Seq[(TopicPartition, Option[LeaderAndIsr], Boolean)]
): Seq[ElectionResult] // (1)!
leaderForOffline(
  partition: TopicPartition,
  leaderAndIsrOpt: Option[LeaderAndIsr],
  uncleanLeaderElectionEnabled: Boolean,
  isLeaderRecoverySupported: Boolean,
  controllerContext: ControllerContext): ElectionResult
```

1. Uses the other `leaderForOffline` for every tuple in `partitionsWithUncleanLeaderRecoveryState`

`leaderForOffline` requests the given [ControllerContext](ControllerContext.md) for the [partition replicas](ControllerContext.md#partitionReplicaAssignment) and removes replicas that are not [online](ControllerContext.md#isReplicaOnline).

For the input `LeaderAndIsr` defined, `leaderForOffline` [offlinePartitionLeaderElection](PartitionLeaderElectionAlgorithms.md#offlinePartitionLeaderElection) (with the replica brokers) and...FIXME

---

`leaderForOffline` is used when:

* `ZkPartitionStateMachine` is requested to [doElectLeaderForPartitions](ZkPartitionStateMachine.md#doElectLeaderForPartitions) (with [OfflinePartitionLeaderElectionStrategy](PartitionStateMachine.md#OfflinePartitionLeaderElectionStrategy))
