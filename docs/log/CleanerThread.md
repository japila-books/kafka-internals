# CleanerThread

`CleanerThread` is a non-daemon thread of execution for [LogCleaner](LogCleaner.md#cleaners) for [log cleanup](#doWork) (one at a time until no more is left).

## Creating Instance

`CleanerThread` takes the following to be created:

* <span id="threadId"> Thread ID

`CleanerThread` is created when:

* `LogCleaner` is requested to [start up](#startup)

## <span id="name"> Thread Name

`CleanerThread` uses the following as the thread name (with the [threadId](#threadId))

```text
kafka-log-cleaner-thread-[threadId]
```

## <span id="log.cleaner.threads"> log.cleaner.threads

The number of `CleanerThreads` (that [LogCleaner](LogCleaner.md#cleaners) uses) is controlled by [log.cleaner.threads](../KafkaConfig.md#log.cleaner.threads) dynamic configuration.

## <span id="doWork"> doWork

```scala
doWork(): Unit
```

`doWork` is part of the `ShutdownableThread` abstraction.

---

`doWork` [tryCleanFilthiestLog](#tryCleanFilthiestLog). If no logs was cleaned up, `doWork` pauses the thread for [log.cleaner.backoff.ms](../KafkaConfig.md#log.cleaner.backoff.ms) millis.

In the end, `doWork` requests the [LogCleanerManager](LogCleaner.md#cleanerManager) to [maintainUncleanablePartitions](LogCleanerManager.md#maintainUncleanablePartitions).

### <span id="tryCleanFilthiestLog"> tryCleanFilthiestLog

```scala
tryCleanFilthiestLog(): Boolean
```

`tryCleanFilthiestLog` [cleanFilthiestLog](#cleanFilthiestLog).

#### <span id="tryCleanFilthiestLog-LogCleaningException"> LogCleaningException

In case of `LogCleaningException`, `tryCleanFilthiestLog` prints out the following WARN message to the logs:

```text
Unexpected exception thrown when cleaning log [log].
Marking its partition ([topicPartition]) as uncleanable
```

`doWork` requests the [LogCleanerManager](LogCleaner.md#cleanerManager) to [maintainUncleanablePartitions](LogCleanerManager.md#maintainUncleanablePartitions) and returns `false`.

### <span id="cleanFilthiestLog"> cleanFilthiestLog

```scala
cleanFilthiestLog(): Boolean
```

`cleanFilthiestLog`...FIXME

## Logging

`CleanerThread` uses [kafka.log.LogCleaner](LogCleaner.md#logging) logger.
