# ZkAdminManager

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
