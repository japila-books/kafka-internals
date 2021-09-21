# RequestHandlerHelper

## Creating Instance

`RequestHandlerHelper` takes the following to be created:

* <span id="requestChannel"> `RequestChannel`
* <span id="quotas"> `QuotaManagers`
* <span id="time"> `Time`
* <span id="logPrefix"> Log Prefix

`RequestHandlerHelper` is created when:

* `ControllerApis` is created (`requestHelper`)
* `KafkaApis` is [created](KafkaApis.md#requestHelper)

## <span id="onLeadershipChange"> onLeadershipChange

```scala
onLeadershipChange(
  groupCoordinator: GroupCoordinator,
  txnCoordinator: TransactionCoordinator,
  updatedLeaders: Iterable[Partition],
  updatedFollowers: Iterable[Partition]): Unit
```

`onLeadershipChange`...FIXME

`onLeadershipChange` is used when:

* `BrokerServer` is requested to [start up](BrokerServer.md#startup)
* `KafkaApis` is requested to [handleLeaderAndIsrRequest](KafkaApis.md#handleLeaderAndIsrRequest)
* `BrokerMetadataListener` is requested to `handleCommits` and `execCommits`
