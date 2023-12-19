# Reconfigurable

`Reconfigurable` is an [extension](#contract) of the [Configurable](../Configurable.md) abstraction for [services](#implementations) that support [Dynamic Configuration](index.md).

## Contract (Subset)

### reconfigurableConfigs { #reconfigurableConfigs }

```java
Set<String> reconfigurableConfigs()
```

See:

* [MetricsReporter](../metrics/MetricsReporter.md#reconfigurableConfigs)

Used when:

* `SslFactory` is requested to `createNewSslEngineFactory`
* `DataPlaneAcceptor` is requested to `validateReconfiguration`
* `DynamicBrokerConfig` is requested to [addReconfigurable](../dynamic-broker-configuration/DynamicBrokerConfig.md#addReconfigurable), [maybeReconfigure](../dynamic-broker-configuration/DynamicBrokerConfig.md#maybeReconfigure), [processListenerReconfigurable](../dynamic-broker-configuration/DynamicBrokerConfig.md#processListenerReconfigurable), [processReconfiguration](../dynamic-broker-configuration/DynamicBrokerConfig.md#processReconfiguration), [reloadUpdatedFilesWithoutConfigChange](../dynamic-broker-configuration/DynamicBrokerConfig.md#reloadUpdatedFilesWithoutConfigChange)
* `DynamicClientQuotaCallback` is requested to `reconfigurableConfigs`
* `DynamicMetricsReporters` is requested to `reconfigurableConfigs`

## Implementations

* `DynamicClientQuotaCallback`
* `DynamicMetricsReporters`
* `KafkaYammerMetrics`
* `ListenerReconfigurable`
* [MetricsReporter](../metrics/MetricsReporter.md)
* `SslFactory`
