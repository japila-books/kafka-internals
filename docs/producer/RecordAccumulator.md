# RecordAccumulator

## Creating Instance

`RecordAccumulator` takes the following to be created:

* <span id="logContext"> LogContext
* <span id="batchSize"> Batch size
* <span id="compression"> CompressionType
* <span id="lingerMs"> Linger
* <span id="retryBackoffMs"> retryBackoffMs
* <span id="deliveryTimeoutMs"> deliveryTimeoutMs
* <span id="metrics"> Metrics
* <span id="metricGrpName"> Name of the Metrics Group
* <span id="time"> Time
* <span id="apiVersions"> ApiVersions
* <span id="transactionManager"> TransactionManager
* <span id="bufferPool"> BufferPool

`RecordAccumulator` is created for [KafkaProducer](KafkaProducer.md#accumulator).

## <span id="appendsInProgress"> appendsInProgress Counter

`RecordAccumulator` creates an `AtomicInteger` ([Java]({{ java.api }}/java/util/concurrent/atomic/AtomicInteger.html)) for `appendsInProgress` internal counter when [created](#creating-instance).

`appendsInProgress` simply marks a single execution of [append](#append) (and is incremented at the beginning and decremented right at the end).

`appendsInProgress` is used when [flushInProgress](#flushInProgress).

### flushInProgress

```java
boolean appendsInProgress()
```

`appendsInProgress` indicates if the [appendsInProgress](#appendsInProgress) counter is above `0`.

`appendsInProgress` is used when [abortIncompleteBatches](#abortIncompleteBatches).

## <span id="flushesInProgress"> flushesInProgress Counter

`RecordAccumulator` creates an `AtomicInteger` ([Java]({{ java.api }}/java/util/concurrent/atomic/AtomicInteger.html)) for `flushesInProgress` internal counter when [created](#creating-instance).

`flushesInProgress` is incremented when [beginFlush](#beginFlush) and decremented when [awaitFlushCompletion](#awaitFlushCompletion).

`flushesInProgress` is used when [flushInProgress](#flushInProgress).

### <span id="flushInProgress"> flushInProgress

```java
boolean flushInProgress()
```

`flushInProgress` indicates if the [flushesInProgress](#flushesInProgress) counter is above `0`.

`flushInProgress` is used when:

* `RecordAccumulator` is requested to [ready](#ready)
* `Sender` is requested to [maybeSendAndPollTransactionalRequest](Sender.md#maybeSendAndPollTransactionalRequest)

## <span id="append"> Appending Record

```java
RecordAppendResult append(
  TopicPartition tp,
  long timestamp,
  byte[] key,
  byte[] value,
  Header[] headers,
  Callback callback,
  long maxTimeToBlock,
  boolean abortOnNewBatch,
  long nowMs)
```

`append`...FIXME

`append` is used when:

* `KafkaProducer` is requested to [send a record](KafkaProducer.md#send) (and [doSend](KafkaProducer.md#doSend))

### <span id="tryAppend"> tryAppend

```java
RecordAppendResult tryAppend(
  long timestamp,
  byte[] key,
  byte[] value,
  Header[] headers,
  Callback callback,
  Deque<ProducerBatch> deque,
  long nowMs)
```

`tryAppend`...FIXME

## <span id="ready"> ready

```java
ReadyCheckResult ready(
  Cluster cluster,
  long nowMs)
```

`ready` is a list of partitions with data ready to send.

`ready`...FIXME

`ready` is used when:

* `Sender` is requested to [sendProducerData](Sender.md#sendProducerData)

## <span id="beginFlush"> beginFlush

```java
void beginFlush()
```

`beginFlush` atomically increments the [flushesInProgress](#flushesInProgress) counter.

`beginFlush` is used when:

* `KafkaProducer` is requested to [flush](KafkaProducer.md#flush)
* `Sender` is requested to [maybeSendAndPollTransactionalRequest](Sender.md#maybeSendAndPollTransactionalRequest)

## <span id="awaitFlushCompletion"> awaitFlushCompletion

```java
void awaitFlushCompletion()
```

`awaitFlushCompletion`...FIXME

`awaitFlushCompletion` is used when:

* `KafkaProducer` is requested to [flush](KafkaProducer.md#flush)

## <span id="splitAndReenqueue"> splitAndReenqueue

```java
int splitAndReenqueue(
  ProducerBatch bigBatch)
```

`splitAndReenqueue`...FIXME

`splitAndReenqueue` is used when:

* `Sender` is requested to [completeBatch](Sender.md#completeBatch)

## <span id="deallocate"> deallocate

```java
void deallocate(
  ProducerBatch batch)
```

`deallocate`...FIXME

`deallocate` is used when:

* `RecordAccumulator` is requested to [abortBatches](#abortBatches) and [abortUndrainedBatches](#abortUndrainedBatches)
* `Sender` is requested to [maybeRemoveAndDeallocateBatch](Sender.md#maybeRemoveAndDeallocateBatch)

## <span id="abortBatches"> abortBatches

```java
void abortBatches() // (1)
void abortBatches(
  RuntimeException reason)
```

1. Uses a `KafkaException`

`abortBatches`...FIXME

`abortBatches` is used when:

* `RecordAccumulator` is requested to [abortIncompleteBatches](#abortIncompleteBatches)
* `Sender` is requested to [maybeAbortBatches](Sender.md#maybeAbortBatches)

## <span id="abortIncompleteBatches"> abortIncompleteBatches

```java
void abortIncompleteBatches()
```

`abortIncompleteBatches` [abortBatches](#abortBatches) as long as there are [appendsInProgress](#appendsInProgress). `abortIncompleteBatches` [abortBatches](#abortBatches) one last time (after no thread was appending in case there was a new batch appended by the last appending thread).

In the end, `abortIncompleteBatches` clears the [batches](#batches) registry.

`abortIncompleteBatches` is used when:

* `Sender` is requested to [run](Sender.md#run) (and [forceClose](Sender.md#forceClose))

## <span id="abortUndrainedBatches"> abortUndrainedBatches

```java
void abortUndrainedBatches(
  RuntimeException reason)
```

`abortUndrainedBatches`...FIXME

`abortUndrainedBatches` is used when:

* `Sender` is requested to [maybeSendAndPollTransactionalRequest](Sender.md#maybeSendAndPollTransactionalRequest)

## <span id="incomplete"> Incomplete (Pending) Batches

`RecordAccumulator` creates an `IncompleteBatches` for `incomplete` internal registry of pending batches when [created](#creating-instance).

`RecordAccumulator` uses the `IncompleteBatches` when:

* [append](#append) (to add a new `ProducerBatch`)
* [splitAndReenqueue](#splitAndReenqueue) (to add a new `ProducerBatch`)
* [deallocate](#deallocate) (to remove a `ProducerBatch`)
* [awaitFlushCompletion](#awaitFlushCompletion), [abortBatches](#abortBatches) and [abortUndrainedBatches](#abortUndrainedBatches) (to copy all `ProducerBatch`s)

### <span id="hasIncomplete"> hasIncomplete

```java
boolean hasIncomplete()
```

`hasIncomplete` is `true` when the [incomplete](#incomplete) registry is not empty.

`hasIncomplete` is used when:

* `Sender` is requested to [maybeSendAndPollTransactionalRequest](Sender.md#maybeSendAndPollTransactionalRequest) and [maybeAbortBatches](Sender.md#maybeAbortBatches)

## <span id="batches"> In-Progress Batches

```java
ConcurrentMap<TopicPartition, Deque<ProducerBatch>> batches
```

`RecordAccumulator` creates a `ConcurrentMap` ([Java]({{ java.api }}/java/util/concurrent/ConcurrentMap.html)) for the `batches` internal registry of in-progress [ProducerBatch](ProducerBatch.md)es (per `TopicPartition`).

`RecordAccumulator` adds a new `ArrayDeque` ([Java]({{ java.api }}/java/util/ArrayDeque.html)) when [getOrCreateDeque](#getOrCreateDeque).

`batches` is used when:

* [expiredBatches](#expiredBatches)
* [ready](#ready)
* [hasUndrained](#hasUndrained)
* [getDeque](#getDeque)
* [batches](#batches)
* [abortIncompleteBatches](#abortIncompleteBatches)

## <span id="getOrCreateDeque"> getOrCreateDeque

```java
Deque<ProducerBatch> getOrCreateDeque(
  TopicPartition tp)
```

`getOrCreateDeque`...FIXME

`getOrCreateDeque` is used when:

* `RecordAccumulator` is requested to [append](#append), [reenqueue](#reenqueue), [splitAndReenqueue](#splitAndReenqueue)
