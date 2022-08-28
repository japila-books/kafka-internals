# DynamicLogConfig

`DynamicLogConfig` is a [BrokerReconfigurable](BrokerReconfigurable.md) of [LogManager](#logManager).

## Creating Instance

`DynamicLogConfig` takes the following to be created:

* <span id="logManager"> [LogManager](../log/LogManager.md)
* <span id="server"> [KafkaBroker](../broker/KafkaBroker.md)

`DynamicLogConfig` is created when:

* `DynamicBrokerConfig` is requested to [register Reconfigurables](DynamicBrokerConfig.md#addReconfigurables)

## <span id="reconfigurableConfigs"><span id="ReconfigurableConfigs"> Reconfigurable Configs

```scala
reconfigurableConfigs: Set[String]
```

`reconfigurableConfigs` is part of the [BrokerReconfigurable](BrokerReconfigurable.md#reconfigurableConfigs) abstraction.

---

`reconfigurableConfigs` returns the values of the [TopicConfigSynonyms](../log/LogConfig.md#TopicConfigSynonyms).

## <span id="reconfigure"> Reconfiguring Broker

```scala
reconfigure(
  oldConfig: KafkaConfig,
  newConfig: KafkaConfig): Unit
```

`reconfigure` is part of the [BrokerReconfigurable](BrokerReconfigurable.md#reconfigure) abstraction.

---

`reconfigure` requests the [LogManager](#logManager) for the [currentDefaultConfig](../log/LogManager.md#currentDefaultConfig) (and the value of [unclean.leader.election.enable](../log/LogConfig.md#uncleanLeaderElectionEnable) configuration property explicitly).

`reconfigure` updates [reconfigurable configuration properties](DynamicLogConfig.md#ReconfigurableConfigs) only.

`reconfigure` requests the [LogManager](#logManager) to [reconfigureDefaultLogConfig](../log/LogManager.md#reconfigureDefaultLogConfig) with the new broker configs.

`reconfigure` [updateLogsConfig](#updateLogsConfig) (with the new broker configs).

In the end, `reconfigure` requests the [KafkaController](../broker/KafkaServer.md#kafkaController) to  [enableDefaultUncleanLeaderElection](../controller/KafkaController.md#enableDefaultUncleanLeaderElection) when [unclean.leader.election.enable](../log/LogConfig.md#uncleanLeaderElectionEnable) is currently enabled while it was not before.
