# LogLoader

## Creating Instance

`LogLoader` takes the following to be created:

* <span id="dir"> Directory
* <span id="topicPartition"> `TopicPartition`
* <span id="config"> [LogConfig](LogConfig.md)
* <span id="scheduler"> `Scheduler`
* <span id="time"> `Time`
* <span id="logDirFailureChannel"> `LogDirFailureChannel`
* <span id="hadCleanShutdown"> `hadCleanShutdown` flag
* <span id="segments"> `LogSegments`
* <span id="logStartOffsetCheckpoint"> `logStartOffsetCheckpoint`
* <span id="recoveryPointCheckpoint"> `recoveryPointCheckpoint`
* <span id="leaderEpochCache"> `LeaderEpochFileCache`
* <span id="producerStateManager"> `ProducerStateManager`
* <span id="numRemainingSegments"> `numRemainingSegments`
* <span id="isRemoteLogEnabled"> `isRemoteLogEnabled` flag (default: `false`)

`LogLoader` is created when:

* `UnifiedLog` is requested to [create a UnifiedLog](UnifiedLog.md#apply)

## Loading { #load }

```scala
load(): LoadedLogOffsets
```

`load`...FIXME

---

`load` is used when:

* `UnifiedLog` is requested to [create a UnifiedLog](UnifiedLog.md#apply)
