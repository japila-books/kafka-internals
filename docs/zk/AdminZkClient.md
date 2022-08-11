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
