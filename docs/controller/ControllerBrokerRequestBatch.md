# ControllerBrokerRequestBatch

`ControllerBrokerRequestBatch` is an [AbstractControllerBrokerRequestBatch](AbstractControllerBrokerRequestBatch.md).

Every time `ControllerBrokerRequestBatch` is used it is first requested to [prepare a new batch](AbstractControllerBrokerRequestBatch.md#newBatch) (of controller requests) that is then [send out to selected brokers](AbstractControllerBrokerRequestBatch.md#sendRequestsToBrokers).

## Creating Instance

`ControllerBrokerRequestBatch` takes the following to be created:

* <span id="config"> [KafkaConfig](../KafkaConfig.md)
* <span id="controllerChannelManager"> `ControllerChannelManager`
* <span id="controllerEventManager"> [ControllerEventManager](ControllerEventManager.md)
* <span id="controllerContext"> [ControllerContext](ControllerContext.md)
* <span id="stateChangeLogger"> `StateChangeLogger`

`ControllerBrokerRequestBatch` is created along with a [KafkaController](KafkaController.md), separately for the following:

* [ControllerBrokerRequestBatch](KafkaController.md#brokerRequestBatch)
* [ZkReplicaStateMachine](KafkaController.md#replicaStateMachine)
* [ZkPartitionStateMachine](KafkaController.md#partitionStateMachine)

## <span id="sendEvent"> Sending ControllerEvent

```scala
sendEvent(
  event: ControllerEvent): Unit
```

`sendEvent` is part of the [AbstractControllerBrokerRequestBatch](AbstractControllerBrokerRequestBatch.md#sendEvent) abstraction.

---

`sendEvent` requests the [ControllerEventManager](#controllerEventManager) to [enqueue](ControllerEventManager.md#put) the input [ControllerEvent](ControllerEvent.md).

## <span id="sendRequest"> Sending Request (to Broker)

```scala
sendRequest(
  brokerId: Int,
  request: AbstractControlRequest.Builder[_ <: AbstractControlRequest],
  callback: AbstractResponse => Unit = null): Unit
```

`sendRequest` is part of the [AbstractControllerBrokerRequestBatch](AbstractControllerBrokerRequestBatch.md#sendRequest) abstraction.

---

`sendRequest` requests the given [ControllerChannelManager](#controllerChannelManager) to [send an controller request out to a broker](ControllerChannelManager.md#sendRequest).
