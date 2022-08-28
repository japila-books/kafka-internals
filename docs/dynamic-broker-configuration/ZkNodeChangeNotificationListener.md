# ZkNodeChangeNotificationListener

## Creating Instance

`ZkNodeChangeNotificationListener` takes the following to be created:

* <span id="zkClient"> [KafkaZkClient](../zk/KafkaZkClient.md)
* <span id="seqNodeRoot"> Node Root
* <span id="seqNodePrefix"> Node Prefix
* <span id="notificationHandler"> `NotificationHandler`
* <span id="changeExpirationMs"> Change Expiration (in millis, default: `15 * 60 * 1000`)
* <span id="time"> `Time`

`ZkNodeChangeNotificationListener` is created when:

* `DelegationTokenManager` is requested to `startup`
* `ZkConfigManager` is [created](ZkConfigManager.md#configChangeListener)
* `ZkAclChangeStore` is requested to `createListener`
