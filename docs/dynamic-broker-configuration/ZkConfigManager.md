# ZkConfigManager

## Creating Instance

`ZkConfigManager` takes the following to be created:

* <span id="zkClient"> [KafkaZkClient](../zk/KafkaZkClient.md)
* <span id="configHandlers"> [ConfigHandler](ConfigHandler.md)s by name

`ZkConfigManager` is created when:

* `KafkaServer` is requested to [start up](../broker/KafkaServer.md#startup) (and initializes the [dynamicConfigManager](../broker/KafkaServer.md#dynamicConfigManager))

## <span id="startup"> Starting Up

```scala
startup(): Unit
```

`startup` begins watching for config changes by requesting the [ZkNodeChangeNotificationListener](#configChangeListener) to [initialize](ZkNodeChangeNotificationListener.md#init).

`startup`...FIXME

---

`startup` is used when:

* `KafkaServer` is requested to [start up](../broker/KafkaServer.md#startup)
