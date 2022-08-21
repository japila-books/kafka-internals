# TopicUncleanLeaderElectionEnable

`TopicUncleanLeaderElectionEnable` is a [ControllerEvent](ControllerEvent.md) that [KafkaController](KafkaController.md) uses to trigger [processTopicUncleanLeaderElectionEnable](KafkaController.md#processTopicUncleanLeaderElectionEnable).

## Creating Instance

`TopicUncleanLeaderElectionEnable` takes the following to be created:

* <span id="topic"> Topic Name

`TopicUncleanLeaderElectionEnable` is created when:

* `KafkaController` is requested to [enableTopicUncleanLeaderElection](KafkaController.md#enableTopicUncleanLeaderElection) (on an active controller)

## <span id="state"> ControllerState

```scala
state: ControllerState
```

`state` is part of the [ControllerEvent](ControllerEvent.md#state) abstraction.

---

`state` is `TopicUncleanLeaderElectionEnable`.

## <span id="KafkaController"> KafkaController


