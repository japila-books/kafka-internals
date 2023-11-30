# LocalLog

## Creating Instance

`LocalLog` takes the following to be created:

* <span id="_dir"> Directory
* <span id="config"> [LogConfig](LogConfig.md)
* <span id="segments"> `LogSegments`
* <span id="recoveryPoint"> Recovery Point
* <span id="nextOffsetMetadata"> `LogOffsetMetadata`
* <span id="scheduler"> `Scheduler`
* <span id="time"> `Time`
* <span id="topicPartition"> `TopicPartition`
* <span id="logDirFailureChannel"> `LogDirFailureChannel`

`LocalLog` is created when:

* `UnifiedLog` is requested to [create a UnifiedLog](UnifiedLog.md#apply)

## truncateFullyAndStartAt { #truncateFullyAndStartAt }

```scala
truncateFullyAndStartAt(
  newOffset: Long): Iterable[LogSegment]
```

`truncateFullyAndStartAt`...FIXME

---

`truncateFullyAndStartAt` is used when:

* `UnifiedLog` is requested to [truncateFullyAndStartAt](UnifiedLog.md#truncateFullyAndStartAt)

## roll { #roll }

```scala
roll(
  expectedNextOffset: Option[Long] = None): LogSegment
```

`roll`...FIXME

---

`roll` is used when:

* `UnifiedLog` is requested to [roll](UnifiedLog.md#roll)

## splitOverflowedSegment { #splitOverflowedSegment }

```scala
splitOverflowedSegment(
  segment: LogSegment,
  existingSegments: LogSegments,
  dir: File,
  topicPartition: TopicPartition,
  config: LogConfig,
  scheduler: Scheduler,
  logDirFailureChannel: LogDirFailureChannel,
  logPrefix: String): SplitSegmentResult
```

`splitOverflowedSegment`...FIXME

---

`splitOverflowedSegment` is used when:

* `UnifiedLog` is requested to [splitOverflowedSegment](UnifiedLog.md#splitOverflowedSegment)

## createNewCleanedSegment { #createNewCleanedSegment }

```scala
createNewCleanedSegment(
  dir: File,
  logConfig: LogConfig,
  baseOffset: Long): LogSegment
```

`createNewCleanedSegment`...FIXME

---

`createNewCleanedSegment` is used when:

* `LocalLog` is requested to [splitOverflowedSegment](#splitOverflowedSegment)
* `UnifiedLog` is requested to [createNewCleanedSegment](UnifiedLog.md#createNewCleanedSegment)

## createAndDeleteSegment { #createAndDeleteSegment }

```scala
createAndDeleteSegment(
  newOffset: Long,
  segmentToDelete: LogSegment,
  asyncDelete: Boolean,
  reason: SegmentDeletionReason): LogSegment
```

`createAndDeleteSegment`...FIXME

---

`createAndDeleteSegment` is used when:

* `LocalLog` is requested to [roll](#roll), [truncateFullyAndStartAt](#truncateFullyAndStartAt)
