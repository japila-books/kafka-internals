# AdminZkClient

## Creating Instance

`AdminZkClient` takes the following to be created:

* <span id="zkClient"> [KafkaZkClient](KafkaZkClient.md)

`AdminZkClient` is created when:

* `ConfigCommand` is requested to `processCommandWithZk`
* `DynamicBrokerConfig` is requested to [initialize](../dynamic-broker-configuration/DynamicBrokerConfig.md#initialize)
* `KafkaServer` is requested to [start up](../broker/KafkaServer.md#startup)
* `ZkAdminManager` is created (`adminZkClient`)
* `ZkConfigManager` is created (`adminZkClient`)
* `ZkConfigRepository` is created (`adminZkClient`)

## <span id="fetchEntityConfig"> fetchEntityConfig

```scala
fetchEntityConfig(
  rootEntityType: String,
  sanitizedEntityName: String): Properties
```

`fetchEntityConfig` requests the [KafkaZkClient](#zkClient) to [getEntityConfigs](KafkaZkClient.md#getEntityConfigs).

---

`fetchEntityConfig` is used when:

* FIXME

## <span id="changeEntityConfig"> changeEntityConfig

```scala
changeEntityConfig(
  rootEntityType: String,
  fullSanitizedEntityName: String,
  configs: Properties): Unit
```

`changeEntityConfig`...FIXME

---

`changeEntityConfig` is used when:

* `AdminZkClient` is requested to [changeClientIdConfig](#changeClientIdConfig), [changeUserOrUserClientIdConfig](#changeUserOrUserClientIdConfig), [changeIpConfig](#changeIpConfig), [changeTopicConfig](#changeTopicConfig), [changeBrokerConfig](#changeBrokerConfig)

## <span id="changeTopicConfig"> changeTopicConfig

```scala
changeTopicConfig(
  topic: String,
  configs: Properties): Unit
```

`changeTopicConfig`...FIXME

---

`changeTopicConfig` is used when:

* `ZkAdminManager` is requested to [alterTopicConfigs](../ZkAdminManager.md#alterTopicConfigs)
* `AdminZkClient` is requested to [changeConfigs](#changeConfigs)

## <span id="changeConfigs"> changeConfigs

```scala
changeConfigs(
  entityType: String,
  entityName: String,
  configs: Properties): Unit
```

`changeConfigs`...FIXME

---

`changeConfigs` is used when:

* `ConfigCommand` is requested to [alterConfigWithZk](../tools/kafka-configs/ConfigCommand.md#alterConfigWithZk)
* `ZkAdminManager` is requested to [alterClientQuotas](../ZkAdminManager.md#alterClientQuotas), [alterUserScramCredentials](../ZkAdminManager.md#alterUserScramCredentials)
