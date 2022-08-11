# LogCleaner

`LogCleaner` is [created](#creating-instance) and [started](#startup) immediately for [LogManager](LogManager.md) when [started](LogManager.md#startupWithConfigOverrides) with [log.cleaner.enable](../KafkaConfig.md#log.cleaner.enable) configuration property enabled.

## Creating Instance

`LogCleaner` takes the following to be created:

* <span id="initialConfig"> [CleanerConfig](CleanerConfig.md)
* <span id="logDirs"> Log directories
* <span id="logs"> Logs (`Pool[TopicPartition, UnifiedLog]`)
* <span id="logDirFailureChannel"> `LogDirFailureChannel`
* <span id="time"> `Time`

`LogCleaner` is created when:

* `LogManager` is requested to [startupWithConfigOverrides](LogManager.md#startupWithConfigOverrides) (with [log.cleaner.enable](../KafkaConfig.md#log.cleaner.enable) enabled)

## <span id="KafkaMetricsGroup"> KafkaMetricsGroup

`LogCleaner` is a [KafkaMetricsGroup](../metrics/KafkaMetricsGroup.md).

## <span id="BrokerReconfigurable"> BrokerReconfigurable

`LogCleaner` is a [BrokerReconfigurable](../dynamic-broker-configuration//BrokerReconfigurable.md).

## <span id="startup"> Starting Up

```scala
startup(): Unit
```

`startup` prints out the following INFO message to the logs:

```text
Starting the log cleaner
```

`startup` creates [log.cleaner.threads](../KafkaConfig.md#logCleanerThreads) of [CleanerThread](CleanerThread.md)s that are immediately [started](CleanerThread.md#doWork).

`startup` adds the `CleanerThread`s to the [cleaners](#cleaners) registry.

---

`startup` is used when:

* `LogCleaner` is requested to [reconfigure](#reconfigure)
* `LogManager` is requested to [startupWithConfigOverrides](LogManager.md#startupWithConfigOverrides) (with [log.cleaner.enable](../KafkaConfig.md#log.cleaner.enable) enabled)

## <span id="cleanerConfig"> Creating CleanerConfig

```scala
cleanerConfig(
  config: KafkaConfig): CleanerConfig
```

`cleanerConfig` creates a [CleanerConfig](CleanerConfig.md) with the configuration properties from the given [KafkaConfig](../KafkaConfig.md).

---

`cleanerConfig` is used when:

* `LogCleaner` is requested to [validateReconfiguration](#validateReconfiguration) and [reconfigure](#reconfigure)
* `LogManager` utility is used to [create a LogManager](LogManager.md#apply)

## <span id="cleaners"> CleanerThreads

`LogCleaner` uses [CleanerThread](CleanerThread.md)s for log cleaning.

`LogCleaner` creates [log.cleaner.threads](../KafkaConfig.md#logCleanerThreads) of [CleanerThread](CleanerThread.md)s when [started up](#startup). They are [shut down](CleanerThread.md#shutdown) when [shutdown](#shutdown).

Use the following metrics to monitor the threads:

* [cleaner-recopy-percent](#cleaner-recopy-percent)
* [DeadThreadCount](#DeadThreadCount)

## Logging

Enable `ALL` logging level for `kafka.log.LogCleaner` logger to see what happens inside.

Add the following line to `config/log4j.properties`:

```text
log4j.logger.kafka.log.LogCleaner=ALL
```

Refer to [Logging](../logging.md).

!!! note
    Kafka comes with a preconfigured `kafka.log.LogCleaner` logger in `config/log4j.properties`:

    ```text
    log4j.appender.cleanerAppender=org.apache.log4j.DailyRollingFileAppender
    log4j.appender.cleanerAppender.DatePattern='.'yyyy-MM-dd-HH
    log4j.appender.cleanerAppender.File=${kafka.logs.dir}/log-cleaner.log
    log4j.appender.cleanerAppender.layout=org.apache.log4j.PatternLayout
    log4j.appender.cleanerAppender.layout.ConversionPattern=[%d] %p %m (%c)%n

    log4j.logger.kafka.log.LogCleaner=INFO, cleanerAppender
    log4j.additivity.kafka.log.LogCleaner=false
    ```

    That means that the logs go to `logs/log-cleaner.log` file at `INFO` logging level and are not added to the main logs (per `log4j.additivity`).
