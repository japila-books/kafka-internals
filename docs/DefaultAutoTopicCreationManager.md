# DefaultAutoTopicCreationManager

`DefaultAutoTopicCreationManager` is a [AutoTopicCreationManager](AutoTopicCreationManager.md).

## Creating Instance

`DefaultAutoTopicCreationManager` takes the following to be created:

* <span id="config"> [KafkaConfig](KafkaConfig.md)
* <span id="channelManager"> Optional `BrokerToControllerChannelManager`
* <span id="adminManager"> Optional `ZkAdminManager`
* <span id="controller"> Optional `KafkaController`
* <span id="groupCoordinator"> `GroupCoordinator`
* [TransactionCoordinator](#txnCoordinator)

`DefaultAutoTopicCreationManager` is created when:

* `AutoTopicCreationManager` utility is used to [create an AutoTopicCreationManager](AutoTopicCreationManager.md#apply)
* `BrokerServer` is requested to [startup](BrokerServer.md#autoTopicCreationManager)

## <span id="txnCoordinator"> TransactionCoordinator

`DefaultAutoTopicCreationManager` is given a [TransactionCoordinator](transactions/TransactionCoordinator.md) when [created](#creating-instance).

`DefaultAutoTopicCreationManager` uses the `TransactionCoordinator` for the [transactionTopicConfigs](transactions/TransactionCoordinator.md#transactionTopicConfigs) when requested to [creatableTopic](#creatableTopic) for [__transaction_state](transactions/index.md#TRANSACTION_STATE_TOPIC_NAME) topic.
