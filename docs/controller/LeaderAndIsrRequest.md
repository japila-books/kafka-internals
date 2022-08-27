# LeaderAndIsrRequest

`LeaderAndIsrRequest` is a [controller request](AbstractControlRequest.md) with `LeaderAndIsr` API key and the following properties:

* <span id="version"> Version
* <span id="controllerId"> Controller ID ([broker.id](../KafkaConfig.md#brokerId) of the controller broker)
* <span id="controllerEpoch"> Controller Epoch
* <span id="brokerEpoch"> Broker Epoch
* <span id="partitionStates"> `PartitionStates` by `TopicPartition` (`Map<TopicPartition, PartitionState>`)
* <span id="topicIds"> Topic IDs
* <span id="liveLeaders"> Live leaders

`LeaderAndIsrRequest` is created (using [LeaderAndIsrRequest.Builder](#build)) when:

* `AbstractRequest` is requested to [parse a request](../AbstractRequest.md#parseRequest) (with the [LeaderAndIsr](#LEADER_AND_ISR) API key)
* `LeaderAndIsrRequest` is requested to [parse a byte buffer](#parse)
* `LeaderAndIsrRequest.Builder` is requested to [build a LeaderAndIsrRequest](#build) (when `ControllerBrokerRequestBatch` is requested to [sendRequestsToBrokers](AbstractControllerBrokerRequestBatch.md#sendRequestsToBrokers))

## <span id="LeaderAndIsrRequest.Builder"><span id="Builder"><span id="build"> LeaderAndIsrRequest.Builder

`LeaderAndIsrRequest` comes with a concrete [AbstractRequest.Builder](../AbstractRequest.md#Builder) factory object that can build a `LeaderAndIsrRequest`.

`LeaderAndIsrRequest.Builder` is used when:

* `AbstractControllerBrokerRequestBatch` is requested to [send out LeaderAndIsr requests to leader and follower brokers](AbstractControllerBrokerRequestBatch.md#sendLeaderAndIsrRequest)
