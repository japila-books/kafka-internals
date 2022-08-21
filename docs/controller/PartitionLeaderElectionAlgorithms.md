# PartitionLeaderElectionAlgorithms

`PartitionLeaderElectionAlgorithms` is a utility with the algorithms for [partition leader election](../partition-leader-election/index.md).

## <span id="offlinePartitionLeaderElection"> offlinePartitionLeaderElection

```scala
offlinePartitionLeaderElection(
  assignment: Seq[Int],
  isr: Seq[Int],
  liveReplicas: Set[Int],
  uncleanLeaderElectionEnabled: Boolean,
  controllerContext: ControllerContext): Option[Int]
```

`offlinePartitionLeaderElection` finds the first broker ID (among the `liveReplicas`) that is among the `isr`.

If not found and `uncleanLeaderElectionEnabled` flag is enabled, `offlinePartitionLeaderElection` finds the first live replica broker (from the `assignment` that is among `liveReplicas`). When successful and a live replica broker is found, `offlinePartitionLeaderElection` marks the occurrence of this unclean leader election event in [kafka.controller:type=ControllerStats,name=UncleanLeaderElectionsPerSec](ControllerContext.md#uncleanLeaderElectionRate) metric.

In the end, `offlinePartitionLeaderElection` returns a broker ID (if a new partition leader is found) or `None`.

---

`offlinePartitionLeaderElection` is used when:

* `Election` is requested to [leaderForOffline](Election.md#leaderForOffline)
