# ConfigHandler

`ConfigHandler` is an [abstraction](#contract) of [config change handlers](#implementations).

## Contract

### <span id="processConfigChanges"> processConfigChanges

```scala
processConfigChanges(
  entityName: String,
  value: Properties): Unit
```

Used when:

* `ConfigChangedNotificationHandler` is requested to `processEntityConfigChangeVersion1`, `processEntityConfigChangeVersion2`
* `ZkConfigManager` is requested to [start up](ZkConfigManager.md#startup)
* `BrokerMetadataPublisher` is requested to `publish`

## Implementations

* [BrokerConfigHandler](BrokerConfigHandler.md)
* `ClientIdConfigHandler`
* `IpConfigHandler`
* [TopicConfigHandler](TopicConfigHandler.md)
* `UserConfigHandler`
