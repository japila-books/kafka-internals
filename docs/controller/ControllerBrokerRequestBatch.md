# ControllerBrokerRequestBatch

## Review Me

`ControllerBrokerRequestBatch` is a concrete <<kafka-controller-AbstractControllerBrokerRequestBatch.adoc#, AbstractControllerBrokerRequestBatch>>.

`ControllerBrokerRequestBatch` uses a <<controllerChannelManager, ControllerChannelManager>> to <<sendRequest, send an controller request out to a broker>>.

`ControllerBrokerRequestBatch` uses a <<controllerEventManager, ControllerEventManager>> to <<sendEvent, enqueue a ControllerEvent (to ControllerEventManager)>>.

`ControllerBrokerRequestBatch` is <<creating-instance, created>> exclusively for <<kafka-controller-KafkaController.adoc#, KafkaController>> (for <<kafka-controller-KafkaController.adoc#brokerRequestBatch, brokerRequestBatch>>, <<kafka-controller-KafkaController.adoc#replicaStateMachine, ReplicaStateMachine>>, and <<kafka-controller-KafkaController.adoc#partitionStateMachine, PartitionStateMachine>>).

Every time `ControllerBrokerRequestBatch` is used it is first requested to <<kafka-controller-AbstractControllerBrokerRequestBatch.adoc#newBatch, prepare a new batch>> (of controller requests) that is then <<kafka-controller-AbstractControllerBrokerRequestBatch.adoc#sendRequestsToBrokers, send out to selected brokers>>.

=== [[creating-instance]] Creating ControllerBrokerRequestBatch Instance

`ControllerBrokerRequestBatch` takes the following to be created:

* [[config]] <<kafka-server-KafkaConfig.adoc#, KafkaConfig>>
* [[controllerChannelManager]] <<kafka-controller-ControllerChannelManager.adoc#, ControllerChannelManager>>
* [[controllerEventManager]] <<kafka-controller-ControllerEventManager.adoc#, ControllerEventManager>>
* [[controllerContext]] <<kafka-controller-ControllerContext.adoc#, ControllerContext>>
* [[stateChangeLogger]] link:kafka-controller-StateChangeLogger.adoc[StateChangeLogger]

=== [[sendEvent]] Emitting (Enqueuing) ControllerEvent -- `sendEvent` Method

[source, scala]
----
sendEvent(event: ControllerEvent): Unit
----

NOTE: `sendEvent` is part of the <<kafka-controller-AbstractControllerBrokerRequestBatch.adoc#sendEvent, AbstractControllerBrokerRequestBatch Contract>> to send a <<kafka-controller-ControllerEvent.adoc#, ControllerEvent>>.

`sendEvent` simply requests the <<controllerEventManager, ControllerEventManager>> to <<kafka-controller-ControllerEventManager.adoc#put, enqueue the controller event>>.

=== [[sendRequest]] Sending AbstractControlRequest Out to Broker -- `sendRequest` Method

[source, scala]
----
sendRequest(
  brokerId: Int,
  request: AbstractControlRequest.Builder[_ <: AbstractControlRequest],
  callback: AbstractResponse => Unit = null): Unit
----

NOTE: `sendRequest` is part of the <<kafka-controller-AbstractControllerBrokerRequestBatch.adoc#sendRequest, AbstractControllerBrokerRequestBatch Contract>> to send a <<kafka-controller-AbstractControlRequest.adoc#, AbstractControlRequest>> to a broker.

`sendRequest` simply requests the <<controllerChannelManager, ControllerChannelManager>> to <<kafka-controller-ControllerChannelManager.adoc#sendRequest, send the request to the broker>>.
