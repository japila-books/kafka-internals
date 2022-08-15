# Controller Broker

**Controller Broker** ([KafkaController](KafkaController.md)) is a Kafka service that runs on every broker in a Kafka cluster, but only one can be [active](#isActive) (_elected_) at any point in time.

The process of promoting a broker to be the active controller is called [Kafka Controller Election](controller-election.md).

!!! quote "Kafka Controller Internals"
    Quoting [Kafka Controller Internals](https://cwiki.apache.org/confluence/display/KAFKA/Kafka+Controller+Internals):

    > In a Kafka cluster, one of the brokers serves as the controller, which is responsible for managing the states of partitions and replicas and for performing administrative tasks like reassigning partitions.

Kafka Controller registers [handlers](KafkaController.md#znode-change-handlers) to be notified about changes in Zookeeper and propagate them across brokers in a Kafka cluster.

## Lifecycle

When [started](KafkaController.md#startup), `KafkaController` emits [Startup](ControllerEvent.md#Startup) controller event. That starts [controller election](KafkaController.md#elect) (on the [controller-event-thread](ControllerEventThread.md)).

During controller election, one `KafkaController` becomes [active](KafkaController.md#isActive) (_elected_) and [onControllerFailover](KafkaController.md#onControllerFailover). The [ControllerContext](KafkaController.md#controllerContext) is built on what is available in Zookeeper.

While in [initializeControllerContext](KafkaController.md#initializeControllerContext), `KafkaController` [updateLeaderAndIsrCache](KafkaController.md#updateLeaderAndIsrCache) (and [reads partition state](../zk/KafkaZkClient.md#getTopicPartitionStates) from `/brokers/topics/[topic]/partitions/[partition]/state` paths in Zookeeper that is then stored as [partitionLeadershipInfo](ControllerContext.md#partitionLeadershipInfo) of the [ControllerContext](KafkaController.md#controllerContext)).
