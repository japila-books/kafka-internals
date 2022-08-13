# KafkaConfig

`KafkaConfig` is configuration properties of a Kafka broker.

## <span id="authorizer.class.name"><span id="AuthorizerClassNameProp"><span id="authorizer"> authorizer.class.name

The fully-qualified name of a class that implements [Authorizer](authorization/Authorizer.md) interface, which is used by the broker for [request authorization](authorization/index.md).

Default: empty

Use `KafkaConfig.authorizerClassName` to access the current value.

Used when:

* `BrokerServer` is requested to [start up](raft/BrokerServer.md#startup)
* `ControllerServer` is requested to [start up](raft/ControllerServer.md#startup)
* `KafkaServer` is requested to [start up](broker/KafkaServer.md#startup)

## <span id="brokerId"><span id="BrokerIdProp"><span id="broker.id"> broker.id

The broker ID of this Kafka server.

Default: `-1`

If unset or negative, a unique broker id will be [generated](broker/KafkaServer.md#getOrGenerateBrokerId) (when `KafkaServer` is requested to [start up](broker/KafkaServer.md#startup)).

To avoid conflicts between zookeeper generated broker id's and user configured broker id's, generated broker ids start from [reserved.broker.max.id](#MaxReservedBrokerIdProp) + 1

Use `KafkaConfig.brokerId` to access the current value.

---

```scala
import kafka.server.KafkaConfig
// For some reason zookeeper.connect is required?!
val m = Map(
  "zookeeper.connect" -> "xxx"
)
val props = new java.util.Properties()
import scala.jdk.CollectionConverters._
props.putAll(m.asJava)
val config = KafkaConfig.fromProps(props)
assert(config.brokerId == -1)
```

## <span id="brokerIdGenerationEnable"><span id="BrokerIdGenerationEnableProp"><span id="broker.id.generation.enable"> broker.id.generation.enable

Enables [broker id generation](broker/KafkaServer.md#generateBrokerId) on a server. When enabled, [reserved.broker.max.id](#MaxReservedBrokerIdProp) should be reviewed.

Default: `true`

Use `brokerIdGenerationEnable` to access the current value.

## <span id="defaultReplicationFactor"><span id="DefaultReplicationFactorProp"><span id="default.replication.factor"> default.replication.factor

## <span id="interBrokerProtocolVersionString"><span id="InterBrokerProtocolVersionProp"><span id="inter.broker.protocol.version"> inter.broker.protocol.version

Specify which version of the inter-broker protocol to use. Typically bumped up after all brokers were upgraded to a new version.

Default: The latest version of `ApiVersion` of the broker

## <span id="logCleanerEnable"><span id="LogCleanerEnableProp"><span id="log.cleaner.enable"> log.cleaner.enable

Enables [LogCleaner](log/LogCleaner.md)

Default: `true`

* Should be enabled if using any topics with a cleanup.policy=compact including the internal offsets topic
* If disabled those topics will not be compacted and continually grow in size.

Used when:

* `CleanerConfig` is [created](log/CleanerConfig.md#enableCleaner)

## <span id="logCleanerThreads"><span id="LogCleanerThreadsProp"><span id="log.cleaner.threads"> log.cleaner.threads

The number of background threads to use for log cleaning (by [LogCleaner](log/LogCleaner.md))

Default: `1`

Must be at least `0`

[Reconfigurable Config](log/LogCleaner.md#ReconfigurableConfigs)

Used when:

* `CleanerConfig` is [created](log/CleanerConfig.md#numThreads)

## <span id="logCleanerBackoffMs"><span id="LogCleanerBackoffMsProp"><span id="log.cleaner.backoff.ms"> log.cleaner.backoff.ms

How long to pause a [CleanerThread](log/CleanerThread.md) (until next log cleaning attempt) when there are no logs to clean

Default: `15 * 1000`

Must be at least `0`

[Reconfigurable Config](log/LogCleaner.md#ReconfigurableConfigs)

Used when:

* `CleanerConfig` is [created](log/CleanerConfig.md#backOffMs)

## <span id="numPartitions"><span id="NumPartitionsProp"><span id="num.partitions"> num.partitions

## <span id="offsetsTopicPartitions"><span id="OffsetsTopicPartitionsProp"><span id="offsets.topic.num.partitions"> offsets.topic.num.partitions

The number of partitions for `__consumer_offsets` offset commit topic (should not change after deployment)

For every partition there is a `GroupCoordinator` [elected](consumer-groups/GroupCoordinator.md#onElection) to handle consumer groups that are "assigned" to this partition.

Default: `50`

Must be at least 1

Use `offsetsTopicPartitions` to access the current value.

Used when:

* `GroupCoordinator` is requested to [create an OffsetConfig](consumer-groups/GroupCoordinator.md#offsetConfig-KafkaConfig)
* `DefaultAutoTopicCreationManager` is requested to [creatableTopic](DefaultAutoTopicCreationManager.md#creatableTopic)
* `KafkaServer` is requested to [start up](broker/KafkaServer.md#startup) (and starts up the [GroupCoordinator](broker/KafkaServer.md#groupCoordinator))
* `BrokerMetadataPublisher` is requested to `initializeManagers` (and starts up the `GroupCoordinator`)

## <span id="offsetsTopicReplicationFactor"><span id="OffsetsTopicReplicationFactorProp"><span id="offsets.topic.replication.factor"> offsets.topic.replication.factor

## <span id="process.roles"><span id="ProcessRolesProp"><span id="parseProcessRoles"><span id="processRoles"><span id="usesSelfManagedQuorum"> process.roles

A comma-separated list of the roles that this process plays in a Kafka cluster:

Supported values:

* `broker`
* `controller`

Default: (empty)

1. When empty, the process requires Zookeeper (runs with Zookeeper).
1. Only applicable for clusters in KRaft (Kafka Raft) mode
1. If used, [controller.quorum.voters](raft/RaftConfig.md#QUORUM_VOTERS_CONFIG) must contain a parseable set of voters
1. [advertised.listeners](#AdvertisedListenersProp) config must not contain KRaft controller listeners from [controller.listener.names](#ControllerListenerNamesProp) when `process.roles` contains `broker` role because Kafka clients that send requests via advertised listeners do not send requests to KRaft controllers -- they only send requests to KRaft brokers
1. If `process.roles` contains `controller` role, the [node.id](KafkaConfig.md#NodeIdProp) must be included in the set of voters [controller.quorum.voters](raft/RaftConfig.md#QUORUM_VOTERS_CONFIG)
1. If `process.roles` contains just the `broker` role, the [node.id](KafkaConfig.md#NodeIdProp) must not be included in the set of voters [controller.quorum.voters](raft/RaftConfig.md#QUORUM_VOTERS_CONFIG)
1. If [controller.listener.names](#ControllerListenerNamesProp) has multiple entries; only the first will be used when `process.roles` is `broker`
1. The advertised listeners ([advertised.listeners](#AdvertisedListenersProp) or [listeners](#ListenersProp)) config must only contain KRaft controller listeners from [controller.listener.names](#ControllerListenerNamesProp) when `process.roles` is `controller`

## <span id="requestTimeoutMs"><span id="RequestTimeoutMsProp"><span id="request.timeout.ms"> request.timeout.ms

[request.timeout.ms](clients/CommonClientConfigs.md#REQUEST_TIMEOUT_MS_CONFIG)

## <span id="transactionAbortTimedOutTransactionCleanupIntervalMs"><span id="TransactionsAbortTimedOutTransactionCleanupIntervalMsProp"><span id="transaction.abort.timed.out.transaction.cleanup.interval.ms"> transaction.abort.timed.out.transaction.cleanup.interval.ms

## <span id="transactionalIdExpirationMs"><span id="TransactionalIdExpirationMsProp"><span id="transactional.id.expiration.ms"> transactional.id.expiration.ms

## <span id="transactionMaxTimeoutMs"><span id="TransactionsMaxTimeoutMsProp"><span id="transaction.max.timeout.ms"> transaction.max.timeout.ms

## <span id="transactionRemoveExpiredTransactionalIdCleanupIntervalMs"><span id="TransactionsRemoveExpiredTransactionalIdCleanupIntervalMsProp"><span id="transaction.remove.expired.transaction.cleanup.interval.ms"> transaction.remove.expired.transaction.cleanup.interval.ms

## <span id="transactionTopicPartitions"><span id="TransactionsTopicPartitionsProp"><span id="transaction.state.log.num.partitions"> transaction.state.log.num.partitions

The number of partitions for the [transaction topic](transactions/index.md#TRANSACTION_STATE_TOPIC_NAME)

Default: 50

Must be at least 1

## <span id="transactionTopicReplicationFactor"><span id="TransactionsTopicReplicationFactorProp"><span id="transaction.state.log.replication.factor"> transaction.state.log.replication.factor

## <span id="transactionTopicSegmentBytes"><span id="TransactionsTopicSegmentBytesProp"><span id="transaction.state.log.segment.bytes"> transaction.state.log.segment.bytes

## <span id="transactionsLoadBufferSize"><span id="TransactionsLoadBufferSizeProp"><span id="transaction.state.log.load.buffer.size"> transaction.state.log.load.buffer.size

## <span id="transactionTopicMinISR"><span id="TransactionsTopicMinISRProp"><span id="transaction.state.log.min.isr"> transaction.state.log.min.isr

## Utilities

### <span id="interBrokerProtocolVersion"> interBrokerProtocolVersion

```scala
interBrokerProtocolVersion: ApiVersion
```

`interBrokerProtocolVersion` creates a `ApiVersion` for the [inter.broker.protocol.version](#interBrokerProtocolVersionString).

### <span id="requiresZookeeper"> requiresZookeeper

```scala
requiresZookeeper: Boolean
```

`requiresZookeeper` is `true` when [process.roles](#processRoles) is empty.
