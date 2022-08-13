# DefaultAutoTopicCreationManager

`DefaultAutoTopicCreationManager` is an [AutoTopicCreationManager](AutoTopicCreationManager.md).

## Creating Instance

`DefaultAutoTopicCreationManager` takes the following to be created:

* <span id="config"> [KafkaConfig](KafkaConfig.md)
* <span id="channelManager"> Optional `BrokerToControllerChannelManager`
* <span id="adminManager"> Optional [ZkAdminManager](ZkAdminManager.md)
* <span id="controller"> Optional [KafkaController](KafkaController.md)
* <span id="groupCoordinator"> [GroupCoordinator](consumer-groups/GroupCoordinator.md)
* [TransactionCoordinator](#txnCoordinator)

`DefaultAutoTopicCreationManager` is created when:

* `AutoTopicCreationManager` utility is used to [create an AutoTopicCreationManager](AutoTopicCreationManager.md#apply)
* `BrokerServer` is requested to [startup](raft/BrokerServer.md#autoTopicCreationManager)

## <span id="txnCoordinator"> TransactionCoordinator

`DefaultAutoTopicCreationManager` is given a [TransactionCoordinator](transactions/TransactionCoordinator.md) when [created](#creating-instance).

`DefaultAutoTopicCreationManager` uses the `TransactionCoordinator` for the [transactionTopicConfigs](transactions/TransactionCoordinator.md#transactionTopicConfigs) when requested to [creatableTopic](#creatableTopic) for [__transaction_state](transactions/index.md#TRANSACTION_STATE_TOPIC_NAME) topic.

## <span id="createTopics"> createTopics

```scala
createTopics(
  topics: Set[String],
  controllerMutationQuota: ControllerMutationQuota,
  metadataRequestContext: Option[RequestContext]): Seq[MetadataResponseTopic]
```

`createTopics` is part of the [AutoTopicCreationManager](AutoTopicCreationManager.md#createTopics) abstraction.

---

`createTopics` [filterCreatableTopics](#filterCreatableTopics) the given `topics`.

`createTopics` [sendCreateTopicRequest](#sendCreateTopicRequest) when the [controller](#controller) is undefined or is not active and the [channelManager](#channelManager) is defined. Otherwise, `createTopics` [createTopicsInZk](#createTopicsInZk).

### <span id="filterCreatableTopics"> filterCreatableTopics

```scala
filterCreatableTopics(
  topics: Set[String]): (Map[String, CreatableTopic], Seq[MetadataResponseTopic])
```

`filterCreatableTopics` splits the `topics` into [CreatableTopic](#creatableTopic)s and with errors (e.g. invalid names or being created).

### <span id="creatableTopic"> creatableTopic

```scala
creatableTopic(
  topic: String): CreatableTopic
```

`creatableTopic` creates a `CreatableTopic` with the given `topic` name.

Topic Name | Number of Partitions | Replication Factor | Configs
-----------|---------------|-------------------|---------
 `__consumer_offsets` | [offsets.topic.num.partitions](KafkaConfig.md#offsetsTopicPartitions) | [offsets.topic.replication.factor](KafkaConfig.md#offsetsTopicReplicationFactor) | [offsetsTopicConfigs](consumer-groups/GroupCoordinator.md#offsetsTopicConfigs)
 `__transaction_state` | [transaction.state.log.num.partitions](KafkaConfig.md#transactionTopicPartitions) | [transaction.state.log.replication.factor](KafkaConfig.md#transactionTopicReplicationFactor) | [transactionTopicConfigs](transactions/TransactionCoordinator.md#transactionTopicConfigs)
 other topics | [num.partitions](KafkaConfig.md#numPartitions) | [default.replication.factor](KafkaConfig.md#defaultReplicationFactor) |

### <span id="createTopicsInZk"> createTopicsInZk

```scala
createTopicsInZk(
  creatableTopics: Map[String, CreatableTopic],
  controllerMutationQuota: ControllerMutationQuota): Seq[MetadataResponseTopic]
```

`createTopicsInZk` requests the [ZkAdminManager](#adminManager) to [create the topics](ZkAdminManager.md#createTopics).
