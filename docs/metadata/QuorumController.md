# QuorumController

## leaderImbalanceCheckIntervalNs { #leaderImbalanceCheckIntervalNs }

`QuorumController` is given`leaderImbalanceCheckIntervalNs` when created.

The interval is used to [maybeScheduleNextBalancePartitionLeaders](#maybeScheduleNextBalancePartitionLeaders).

## maybeScheduleNextBalancePartitionLeaders { #maybeScheduleNextBalancePartitionLeaders }

```java
void maybeScheduleNextBalancePartitionLeaders()
```

`maybeScheduleNextBalancePartitionLeaders`...FIXME

---

`maybeScheduleNextBalancePartitionLeaders` is used when:

* `CompleteActivationEvent` is requested to `processBatchEndOffset`
* `ControllerWriteEvent` is requested to `run`
