# QueuedEvent

`QueuedEvent` is a [ControllerEvent](#event) with the [time it was enqueued](#enqueueTimeMs) to [ControllerEventManager](ControllerEventManager.md).

## Creating Instance

`QueuedEvent` takes the following to be created:

* <span id="event"> [ControllerEvent](ControllerEvent.md)
* <span id="enqueueTimeMs"> Enqueue time (in millis)

`QueuedEvent` is created when:

* `ControllerEventManager` is requested to [enqueue a ControllerEvent](ControllerEventManager.md#put)

## <span id="process"> Processing ControllerEventProcessor

```scala
process(
  processor: ControllerEventProcessor): Unit
```

`process` requests the input [ControllerEventProcessor](ControllerEventProcessor.md) to [process](ControllerEventProcessor.md#process) the [ControllerEvent](#event).

---

`process` is used when:

* `ControllerEventThread` is requested to [doWork](ControllerEventThread.md#doWork)

## <span id="toString"> String (Textual) Representation

```scala
toString: String
```

`toString` is part of the [java.lang.Object](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/Object.html#toString()) abstraction.

---

`toString` returns the following string representation (with the [ControllerEvent](#event) and the [enqueue time](#enqueueTimeMs)):

```text
QueuedEvent(event=[event], enqueueTimeMs=[enqueueTimeMs])
```
