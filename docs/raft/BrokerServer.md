# BrokerServer

`BrokerServer` is a [KafkaBroker](../KafkaBroker.md) that runs in [KRaft mode](index.md).

## Creating Instance

`BrokerServer` takes the following to be created:

* <span id="config"> [KafkaConfig](../KafkaConfig.md)
* <span id="metaProps"> [MetaProperties](MetaProperties.md)
* <span id="raftManager"> [RaftManager](RaftManager.md)
* <span id="time"> `Time`
* <span id="metrics"> [Metrics](../metrics/Metrics.md)
* <span id="threadNamePrefix"> Optional `threadNamePrefix`
* [Initial Offline Log Directories](#initialOfflineDirs)
* <span id="controllerQuorumVotersFuture"> `controllerQuorumVotersFuture` (`CompletableFuture[util.Map[Integer, AddressSpec]]`)
* <span id="supportedFeatures"> Supported Features

`BrokerServer` is created when:

* `KafkaRaftServer` is [created](KafkaRaftServer.md#broker) (with `BrokerRole` among the [processRoles](../KafkaConfig.md#processRoles))

### <span id="initialOfflineDirs"> Initial Offline Log Directories

```scala
initialOfflineDirs: Seq[String]
```

`BrokerServer` is given `initialOfflineDirs` when [created](#creating-instance) (that is [offlineDirs](KafkaRaftServer.md#offlineDirs)).

`initialOfflineDirs` is used to [create a LogManager](#logManager) when `BrokerServer` is requested to [startup](#startup).

## <span id="startup"> startup

```scala
startup(): Unit
```

`startup`...FIXME

`startup` is used when:

* `KafkaRaftServer` is requested to [startup](KafkaRaftServer.md#startup)
