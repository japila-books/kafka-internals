# ControllerServer

`ControllerServer` represents the `controller` in [process.roles](../KafkaConfig.md#process.roles).

## Creating Instance

`ControllerServer` takes the following to be created:

* <span id="sharedServer"> [SharedServer](SharedServer.md)
* <span id="configSchema"> `KafkaConfigSchema`
* <span id="bootstrapMetadata"> `BootstrapMetadata`

`ControllerServer` is created alongside a [KafkaRaftServer](KafkaRaftServer.md#controller) (for `controller` in [process.roles](../KafkaConfig.md#process.roles)).

## Starting Up { #startup }

```scala
startup(): Unit
```

`startup` changes status from `SHUTDOWN` to `STARTING`.

`startup` uses [server.max.startup.time.ms](../KafkaConfig.md#server.max.startup.time.ms) for...FIXME

`startup` prints out the following INFO message to the logs:

```text
Starting controller
```

`startup` requests the [DynamicBrokerConfig](../KafkaConfig.md#dynamicConfig) (of the [KafkaConfig](#config)) to [initialize](../dynamic-broker-configuration/DynamicBrokerConfig.md#initialize) (with no `KafkaZkClient` as it runs in Zookeeper-less KRaft mode).

`startup` changes status from `STARTING` to `STARTED`.

`startup` registers new metrics (gauges) in the [KafkaMetricsGroup](#metricsGroup).

Metric Name  | Description
-------------|------------
 `ClusterId` | [clusterId](#clusterId)
 `yammer-metrics-count` |
 `linux-disk-read-bytes` | (only on Linux)
 `linux-disk-write-bytes` | (only on Linux)

`startup`...FIXME

!!! note
    There is a lot services being registered that seem not necessarily as important at this early stage of the KRaft exploration of mine 😉

`startup` requests the [SharedServer](#sharedServer) to [startForController](SharedServer.md#startForController).

`startup`...FIXME

`startup` builds the [QuorumController](#controller).

`startup`...FIXME

In the end, `startup` requests the [DynamicBrokerConfig](../KafkaConfig.md#dynamicConfig) 

`startup` [registers](../dynamic-broker-configuration/DynamicBrokerConfig.md#addReconfigurables) this `ControllerServer` for dynamic config changes (to the [KafkaConfig](#config)).

---

`startup` is used when:

* `KafkaRaftServer` is requested to [startup](KafkaRaftServer.md#startup)

## Logging

Enable `ALL` logging level for `kafka.server.ControllerServer` logger to see what happens inside.

Add the following line to `config/log4j.properties`:

```text
log4j.logger.kafka.server.ControllerServer=ALL
```

Refer to [Logging](../logging.md).
