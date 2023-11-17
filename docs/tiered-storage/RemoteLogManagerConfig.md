# RemoteLogManagerConfig

## Creating Instance

`RemoteLogManagerConfig` takes the following to be created:

* <span id="config"> [AbstractConfig](../AbstractConfig.md)

`RemoteLogManagerConfig` is created when:

* `KafkaConfig` is [created](../KafkaConfig.md#_remoteLogManagerConfig)

## <span id="REMOTE_LOG_STORAGE_SYSTEM_ENABLE_PROP"> remote.log.storage.system.enable { #remote.log.storage.system.enable }

**remote.log.storage.system.enable**

Enables [Tiered Storage](index.md)

Default: `false`

Available as [KafkaConfig.isRemoteLogStorageSystemEnabled](../KafkaConfig.md#isRemoteLogStorageSystemEnabled)

Used when:

* `LogManager` is [created](../log/LogManager.md#apply)
* `TopicConfigHandler` is requested to [updateLogConfig](../dynamic-broker-configuration/TopicConfigHandler.md#updateLogConfig)
* `ControllerConfigurationValidator` is requested to `validate`
* `AdminZkClient` is requested to [validateTopicCreate](../zk/AdminZkClient.md#validateTopicCreate) and [validateTopicConfig](../zk/AdminZkClient.md#validateTopicConfig)
