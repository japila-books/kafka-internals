# AutoPreferredReplicaLeaderElection

## <span id="state"> State

`AutoPreferredReplicaLeaderElection` is a [ControllerEvent](../ControllerEvent.md) that [transition](../ControllerEvent.md#state) the [KafkaController](../KafkaController.md) to `AutoLeaderBalance` state.

`AutoPreferredReplicaLeaderElection` is [enqueued](../ControllerEventManager.md#put) (to the [ControllerEventManager](../KafkaController.md#eventManager)) exclusively from the <<kafka-server-scheduled-tasks.md#auto-leader-rebalance-task, auto-leader-rebalance-task>>.

## <span id="process"> Process

When [processed](../ControllerEvent.md#process) on a [controller broker](../KafkaController.md#isActive), `AutoPreferredReplicaLeaderElection` event [checkAndTriggerAutoLeaderRebalance](../KafkaController.md#checkAndTriggerAutoLeaderRebalance) and in the end [scheduleAutoLeaderRebalanceTask](../KafkaController.md#scheduleAutoLeaderRebalanceTask) with the delay based on [leader.imbalance.check.interval.seconds](../../KafkaConfig.md#leader.imbalance.check.interval.seconds) configuration property.

!!! note
    `AutoPreferredReplicaLeaderElection` event is ignored (skipped) when processed on any broker but [controller broker](../KafkaController.md#isActive).
