# ControllerEventProcessor

`ControllerEventProcessor` is an [abstraction](#contract) of [processors](#implementations) that can [process](#process) and [preempt](#preempt) controller events.

## Contract

### <span id="preempt"> preempt

```scala
preempt(
  event: ControllerEvent): Unit
```

Preempts a [ControllerEvent](ControllerEvent.md)

Used when:

* `QueuedEvent` is requested to [preempt a ControllerEventProcessor](QueuedEvent.md#preempt)

### <span id="process"> process

```scala
process(
  event: ControllerEvent): Unit
```

Processes a [ControllerEvent](ControllerEvent.md)

Used when:

* `QueuedEvent` is requested to [process a ControllerEventProcessor](QueuedEvent.md#process)

## Implementations

* [KafkaController](KafkaController.md)
