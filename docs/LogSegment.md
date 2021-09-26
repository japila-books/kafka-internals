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

## <span id="open"> Opening LogSegment

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

`open` is used when:

* `Log` is requested to [roll a log segment](Log.md#roll) and [truncateFullyAndStartAt](Log.md#truncateFullyAndStartAt)
* `LogCleaner` is requested to `createNewCleanedSegment`
* `LogLoader` is requested to `load`, `loadSegmentFiles` and `recoverLog`

## <span id="read"> Reading Messages

```scala
read(
  startOffset: Long,
  maxSize: Int,
  maxPosition: Long = size,
  minOneMessage: Boolean = false): FetchDataInfo
```

`read`...FIXME

`read` is used when:

* `Log` is requested to [read](Log.md#read)
* `LogSegment` is requested to [readNextOffset](#readNextOffset)
