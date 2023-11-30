# LogSegment

## Creating Instance

`LogSegment` takes the following to be created:

* <span id="log"> `FileRecords`
* <span id="lazyOffsetIndex"> `OffsetIndex`
* <span id="lazyTimeIndex"> `TimeIndex`
* <span id="txnIndex"> `TransactionIndex`
* <span id="baseOffset"> baseOffset
* <span id="indexIntervalBytes"> indexIntervalBytes
* <span id="rollJitterMs"> rollJitterMs
* <span id="time"> `Time`

`LogSegment` is created using [open](#open) utility.

## Opening LogSegment { #open }

```scala
open(
  dir: File,
  baseOffset: Long,
  config: LogConfig,
  time: Time,
  fileAlreadyExists: Boolean = false,
  initFileSize: Int = 0,
  preallocate: Boolean = false,
  fileSuffix: String = ""): LogSegment
```

`open`...FIXME

---

`open` is used when:

* `LocalLog` is requested to [createAndDeleteSegment](LocalLog.md#createAndDeleteSegment), [roll](LocalLog.md#roll), [createNewCleanedSegment](LocalLog.md#createNewCleanedSegment)
* `LogLoader` is requested to [load](LogLoader.md#load), [loadSegmentFiles](LogLoader.md#loadSegmentFiles), [recoverLog](LogLoader.md#recoverLog)

## Reading Messages { #read }

```scala
read(
  startOffset: Long,
  maxSize: Int,
  maxPosition: Long = size,
  minOneMessage: Boolean = false): FetchDataInfo
```

`read`...FIXME

---

`read` is used when:

* `Log` is requested to [read](../Log.md#read)
* `LogSegment` is requested to [readNextOffset](#readNextOffset)
