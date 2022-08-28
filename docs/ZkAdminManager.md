# ZkAdminManager

## <span id="incrementalAlterConfigs"> incrementalAlterConfigs

```scala
incrementalAlterConfigs(
  configs: Map[ConfigResource,
  Seq[AlterConfigOp]],
  validateOnly: Boolean): Map[ConfigResource, ApiError]
```

`incrementalAlterConfigs`...FIXME

---

`incrementalAlterConfigs` is used when:

* `KafkaApis` is requested to [processIncrementalAlterConfigsRequest](KafkaApis.md#processIncrementalAlterConfigsRequest)

## <span id="alterConfigs"> alterConfigs

```scala
alterConfigs(
  configs: Map[ConfigResource,
  AlterConfigsRequest.Config],
  validateOnly: Boolean): Map[ConfigResource, ApiError]
```

`alterConfigs`...FIXME

---

`alterConfigs` is used when:

* `KafkaApis` is requested to [processLegacyAlterConfigsRequest](KafkaApis.md#processLegacyAlterConfigsRequest)

## <span id="alterTopicConfigs"> alterTopicConfigs

```scala
alterTopicConfigs(
  resource: ConfigResource,
  validateOnly: Boolean,
  configProps: Properties,
  configEntriesMap: Map[String, String]): (ConfigResource, ApiError)
```

`alterTopicConfigs`...FIXME

---

`alterTopicConfigs` is used when:

* `ZkAdminManager` is requested to [alterConfigs](#alterConfigs) (of a topic), [incrementalAlterConfigs](#incrementalAlterConfigs) (of a topic)

## <span id="createTopics"> Creating Topics

```scala
createTopics(
  timeout: Int,
  validateOnly: Boolean,
  toCreate: Map[String, CreatableTopic],
  includeConfigsAndMetadata: Map[String, CreatableTopicResult],
  controllerMutationQuota: ControllerMutationQuota,
  responseCallback: Map[String, ApiError] => Unit): Unit
```

`createTopics`...FIXME

---

`createTopics` is used when:

* `DefaultAutoTopicCreationManager` is requested to [createTopicsInZk](DefaultAutoTopicCreationManager.md#createTopicsInZk)
* `KafkaApis` is requested to [handleCreateTopicsRequest](KafkaApis.md#handleCreateTopicsRequest)
