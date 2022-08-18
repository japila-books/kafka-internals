# ZkReplicaStateMachine

`ZkReplicaStateMachine` is a [ReplicaStateMachine](ReplicaStateMachine.md) to [handle changes of the state of partition replicas](#handleStateChanges).

`ZkReplicaStateMachine` uses [ControllerBrokerRequestBatch](#controllerBrokerRequestBatch) to propagate _replica state changes_ to all brokers in a Kafka cluster.

## Creating Instance

`ZkReplicaStateMachine` takes the following to be created:

* <span id="config"> [KafkaConfig](../KafkaConfig.md)
* <span id="stateChangeLogger"> `StateChangeLogger`
* <span id="controllerContext"> [ControllerContext](ControllerContext.md)
* <span id="zkClient"> [KafkaZkClient](../zk/KafkaZkClient.md)
* <span id="controllerBrokerRequestBatch"> [ControllerBrokerRequestBatch](ControllerBrokerRequestBatch.md)

`ZkReplicaStateMachine` is created along with a [KafkaController](KafkaController.md#replicaStateMachine).

## <span id="handleStateChanges"> Handling Replica State Changes

```scala
handleStateChanges(
  replicas: Seq[PartitionAndReplica],
  targetState: ReplicaState): Unit
```

`handleStateChanges` is part of the [ReplicaStateMachine](ReplicaStateMachine.md#handleStateChanges) abstraction.

---

!!! note
    `handleStateChanges` is a _noop_ and does nothing when the input `replicas` collection is empty.

`handleStateChanges` requests the [ControllerBrokerRequestBatch](#controllerBrokerRequestBatch) for a [new batch](ControllerBrokerRequestBatch.md#newBatch).

`handleStateChanges` groups the `replicas` by the replica ID and [doHandleStateChanges](#doHandleStateChanges) for every replica ID (with the `ReplicaState`).

In the end, `handleStateChanges` requests the [ControllerBrokerRequestBatch](#controllerBrokerRequestBatch) to [sendRequestsToBrokers](ControllerBrokerRequestBatch.md#sendRequestsToBrokers).

### <span id="doHandleStateChanges"> doHandleStateChanges

```scala
doHandleStateChanges(
  replicaId: Int,
  replicas: Seq[PartitionAndReplica],
  targetState: ReplicaState): Unit
```

For every replica (in the `replicas`), `doHandleStateChanges` requests the [ControllerBrokerRequestBatch](#controllerBrokerRequestBatch) to [putReplicaStateIfNotExists](ControllerBrokerRequestBatch.md#putReplicaStateIfNotExists) (with `NonExistentReplica` state)

`doHandleStateChanges` requests the [ControllerBrokerRequestBatch](#controllerBrokerRequestBatch) to [checkValidReplicaStateChange](ControllerBrokerRequestBatch.md#checkValidReplicaStateChange) (that gives valid and invalid replicas).

For every invalid replica, `doHandleStateChanges` [logInvalidTransition](#logInvalidTransition).

`doHandleStateChanges` branches off per the input target state (`ReplicaState`):

* `NewReplica`
* `OnlineReplica`
* `OfflineReplica`
* `ReplicaDeletionStarted`
* `ReplicaDeletionIneligible`
* `ReplicaDeletionSuccessful`
* `NonExistentReplica`

## Logging

Enable `ALL` logging level for `kafka.controller.ZkReplicaStateMachine` logger to see what happens inside.

Add the following line to `config/log4j.properties`:

```text
log4j.logger.kafka.controller.ZkReplicaStateMachine=ALL
```

Refer to [Logging](../logging.md).

### <span id="logIdent"> logIdent

`ZkReplicaStateMachine` uses the following logging prefix (with the [broker.id](../KafkaConfig.md#brokerId)):

```text
[ReplicaStateMachine controllerId=[brokerId]]
```
