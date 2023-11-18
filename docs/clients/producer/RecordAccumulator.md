# RecordAccumulator

`RecordAccumulator` uses [BufferPool](#free).

## Creating Instance

`RecordAccumulator` takes the following to be created:

* <span id="logContext"> LogContext
* <span id="batchSize"> [batch.size](ProducerConfig.md#BATCH_SIZE_CONFIG)
* <span id="compression"> CompressionType
* <span id="lingerMs"> [linger.ms](ProducerConfig.md#LINGER_MS_CONFIG)
* <span id="retryBackoffMs"> [retry.backoff.ms](ProducerConfig.md#RETRY_BACKOFF_MS_CONFIG)
* <span id="deliveryTimeoutMs"> [configureDeliveryTimeout](KafkaProducer.md#configureDeliveryTimeout)
* <span id="metrics"> `Metrics`
* <span id="metricGrpName"> Name of the Metrics Group
* <span id="time"> `Time`
* <span id="apiVersions"> `ApiVersions`
* [TransactionManager](#transactionManager)
* <span id="bufferPool"> [BufferPool](BufferPool.md)

`RecordAccumulator` is created along with [KafkaProducer](KafkaProducer.md#accumulator).

### BufferPool { #free }

`RecordAccumulator` is given a [BufferPool](BufferPool.md) when [created](#creating-instance).

All the [metrics](#metrics) of `RecordAccumulator` report performance of the `BufferPool`.

The `BufferPool` is used for the following:

* [Adding a record](#append)
* [partitionReady](#partitionReady)
* [deallocate](#deallocate)
* [bufferPoolAvailableMemory](#bufferPoolAvailableMemory)

The `BufferPool` is closed while `RecordAccumulator` is requested to [close](#close).

## Metrics

`RecordAccumulator` registers the metrics under the [producer-metrics](KafkaProducer.md#PRODUCER_METRIC_GROUP_NAME) group name.

### buffer-available-bytes { #buffer-available-bytes }

The total amount of buffer memory that is not being used (either unallocated or in the free list)

[availableMemory](BufferPool.md#availableMemory) of the [BufferPool](#free)

### buffer-total-bytes { #buffer-total-bytes }

The maximum amount of buffer memory the client can use (whether or not it is currently used)

[totalMemory](BufferPool.md#totalMemory) of the [BufferPool](#free)

### waiting-threads { #waiting-threads }

The number of user threads blocked waiting for buffer memory to enqueue their records

[queued](BufferPool.md#queued) of the [BufferPool](#free)

## <span id="transactionManager"> TransactionManager

`RecordAccumulator` is given a [TransactionManager](TransactionManager.md) when [created](#creating-instance).

`RecordAccumulator` uses the `TransactionManager` when requested for the following:

* [reenqueue](#reenqueue)
* [splitAndReenqueue](#splitAndReenqueue)
* [insertInSequenceOrder](#insertInSequenceOrder)
* [drain](#drain) ([drainBatchesForOneNode](#drainBatchesForOneNode) and [shouldStopDrainBatchesForPartition](#shouldStopDrainBatchesForPartition))
* [abortUndrainedBatches](#abortUndrainedBatches)

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

## <span id="reenqueue"> reenqueue

```java
void reenqueue(
  ProducerBatch batch,
  long now)
```

`reenqueue`...FIXME

`reenqueue` is used when:

* `Sender` is requested to [reenqueueBatch](Sender.md#reenqueueBatch)

## <span id="insertInSequenceOrder"> insertInSequenceOrder

```java
void insertInSequenceOrder(
  Deque<ProducerBatch> deque,
  ProducerBatch batch)
```

`insertInSequenceOrder`...FIXME

`insertInSequenceOrder` is used when:

* `RecordAccumulator` is requested to [reenqueue](#reenqueue) and [splitAndReenqueue](#splitAndReenqueue)

## <span id="drain"> drain

```java
Map<Integer, List<ProducerBatch>> drain(
  Cluster cluster,
  Set<Node> nodes,
  int maxSize,
  long now)
```

`drain`...FIXME

`drain` is used when:

* `Sender` is requested to [sendProducerData](Sender.md#sendProducerData)

### <span id="drainBatchesForOneNode"> drainBatchesForOneNode

```java
List<ProducerBatch> drainBatchesForOneNode(
  Cluster cluster,
  Node node,
  int maxSize,
  long now)
```

`drainBatchesForOneNode`...FIXME

### <span id="shouldStopDrainBatchesForPartition"> shouldStopDrainBatchesForPartition

```java
boolean shouldStopDrainBatchesForPartition(
  ProducerBatch first,
  TopicPartition tp)
```

`shouldStopDrainBatchesForPartition`...FIXME

## registerMetrics { #registerMetrics }

```java
void registerMetrics(
  Metrics metrics,
  String metricGrpName)
```

`registerMetrics` registers (_adds_) the [metrics](#metrics) to the given [Metrics](../../metrics/Metrics.md).

---

`registerMetrics` is used when:

* `RecordAccumulator` is [created](#creating-instance)
