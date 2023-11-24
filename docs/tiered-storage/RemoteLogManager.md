# RemoteLogManager

## Creating Instance

`RemoteLogManager` takes the following to be created:

* <span id="rlmConfig"> [RemoteLogManagerConfig](RemoteLogManagerConfig.md)
* <span id="brokerId"> Broker ID
* <span id="logDir"> Log Directory
* <span id="clusterId"> Cluster ID
* <span id="time"> `Time`
* <span id="fetchLog"> Fetch Log Function (`Function<TopicPartition, Optional<UnifiedLog>>`)
* <span id="updateRemoteLogStartOffset"> `updateRemoteLogStartOffset` (`BiConsumer<TopicPartition, Long>`)
* <span id="brokerTopicStats"> `BrokerTopicStats`

`RemoteLogManager` is created when:

* `BrokerServer` is requested to [create a RemoteLogManager](../kraft/BrokerServer.md#createRemoteLogManager)
* `KafkaServer` is requested to [create a RemoteLogManager](../broker/KafkaServer.md#createRemoteLogManager)
