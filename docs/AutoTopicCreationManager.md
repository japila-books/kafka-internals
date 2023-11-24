# AutoTopicCreationManager

`AutoTopicCreationManager` is an [abstraction](#contract) of [managers](#implementations) that can [create topics](#createTopics).

## Contract

### <span id="createTopics"> createTopics

```scala
createTopics(
  topicNames: Set[String],
  controllerMutationQuota: ControllerMutationQuota): Seq[MetadataResponseTopic]
```

Used when:

* `KafkaApis` is requested to [getTopicMetadata](KafkaApis.md#getTopicMetadata) and [handleFindCoordinatorRequest](KafkaApis.md#handleFindCoordinatorRequest)

### <span id="shutdown"> shutdown

```scala
shutdown(): Unit
```

Used when:

* `BrokerServer` is requested to [shutdown](kraft/BrokerServer.md#shutdown)
* `KafkaServer` is requested to [shutdown](broker/KafkaServer.md#shutdown)

### <span id="start"> start

```scala
start(): Unit
```

Used when:

* `BrokerServer` is requested to [startup](kraft/BrokerServer.md#startup)
* `KafkaServer` is requested to [startup](broker/KafkaServer.md#startup)

## Implementations

* [DefaultAutoTopicCreationManager](DefaultAutoTopicCreationManager.md)

## <span id="apply"> Creating AutoTopicCreationManager

```scala
apply(
  config: KafkaConfig,
  metadataCache: MetadataCache,
  time: Time,
  metrics: Metrics,
  threadNamePrefix: Option[String],
  adminManager: Option[ZkAdminManager],
  controller: Option[KafkaController],
  groupCoordinator: GroupCoordinator,
  txnCoordinator: TransactionCoordinator,
  enableForwarding: Boolean): AutoTopicCreationManager
```

`apply` creates a [DefaultAutoTopicCreationManager](DefaultAutoTopicCreationManager.md).

`apply` is used when:

* `KafkaServer` is requested to [startup](broker/KafkaServer.md#autoTopicCreationManager)
