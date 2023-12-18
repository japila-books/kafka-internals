# KafkaConfig

`KafkaConfig` is configuration properties of a Kafka broker.

`KafkaConfig` is a [AbstractConfig](AbstractConfig.md).

## Accessing KafkaConfig

```scala
import kafka.server.KafkaConfig
import java.util.Properties
val props = new Properties()
props.put("zookeeper.connect", ":2181") // a required property
val config = KafkaConfig.fromProps(props, doLog = true)
assert(config.uncleanLeaderElectionEnable == false)
```

## <span id="authorizer.class.name"><span id="AuthorizerClassNameProp"><span id="authorizer"> authorizer.class.name

The fully-qualified name of a class that implements [Authorizer](authorization/Authorizer.md) interface, which is used by the broker for [request authorization](authorization/index.md).

Default: empty

Use `KafkaConfig.authorizerClassName` to access the current value.

Used when:

* `BrokerServer` is requested to [start up](kraft/BrokerServer.md#startup)
* `ControllerServer` is requested to [start up](kraft/ControllerServer.md#startup)
* `KafkaServer` is requested to [start up](broker/KafkaServer.md#startup)

## <span id="AutoLeaderRebalanceEnableProp"><span id="autoLeaderRebalanceEnable"> auto.leader.rebalance.enable { #auto.leader.rebalance.enable }

Enables auto partition leader balancing.
A background thread checks the distribution of partition leaders at regular intervals ([leader.imbalance.check.interval.seconds](#leader.imbalance.check.interval.seconds)).
If the leader imbalance exceeds [leader.imbalance.per.broker.percentage](#leader.imbalance.per.broker.percentage), leader rebalance to the preferred leader for partitions is triggered.

Default: `true`

Importance: High

Used when:

* `KafkaController` is requested to [onControllerFailover](controller/KafkaController.md#onControllerFailover)
* `ControllerServer` is requested to [startup](kraft/ControllerServer.md#startup)

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

## <span id="metadata.log.dir"><span id="MetadataLogDirProp"> metadata.log.dir { #metadataLogDir }

The directory of the metadata log of a Kafka cluster in [KRaft mode](kraft/index.md).

Unless specified, the metadata log is placed in the [first log directory from log.dirs](#metadataLogDir).

Default: (unspecified)

Available as [metadataLogDir](#metadataLogDir)

## <span id="metadata.log.max.record.bytes.between.snapshots"><span id="MetadataSnapshotMaxNewRecordBytesProp"> metadata.log.max.record.bytes.between.snapshots { #metadataSnapshotMaxNewRecordBytes }

The maximum number of bytes in the log between the latest snapshot and the high-watermark needed before generating a new snapshot.

To generate snapshots based on the time elapsed use [metadata.log.max.snapshot.interval.ms](#metadata.log.max.snapshot.interval.ms) configuration.

The Kafka node will generate a snapshot when either the maximum time interval is reached or the maximum bytes limit is reached.

Default: `20 * 1024 * 1024`

At least `1`

Priority: `HIGH`

Available as `KafkaConfig.metadataSnapshotMaxNewRecordBytes`

Used when:

* `SharedServer` is requested to [start](kraft/SharedServer.md#snapshotGenerator)

## <span id="metadata.log.max.snapshot.interval.ms"><span id="MetadataSnapshotMaxIntervalMsProp"> metadata.log.max.snapshot.interval.ms { #metadataSnapshotMaxIntervalMs }

The maximum number of milliseconds to wait to generate a snapshot if there are committed records in the log that are not included in the latest snapshot.

A value of zero disables time-based snapshot generation.

To generate snapshots based on the number of metadata bytes use [metadata.log.max.record.bytes.between.snapshots](#metadata.log.max.record.bytes.between.snapshots) configuration.

The Kafka node will generate a snapshot when either the maximum time interval is reached or the maximum bytes limit is reached.

Default: `1` hour

At least `0`

Priority: `HIGH`

Available as `KafkaConfig.metadataSnapshotMaxIntervalMs`

Used when:

* `SharedServer` is requested to [start](kraft/SharedServer.md#snapshotGenerator)

## <span id="numPartitions"><span id="NumPartitionsProp"><span id="num.partitions"> num.partitions

## <span id="offsetsTopicPartitions"><span id="OffsetsTopicPartitionsProp"><span id="offsets.topic.num.partitions"> offsets.topic.num.partitions

The number of partitions for `__consumer_offsets` offset commit topic (should not change after deployment)

For every partition there is a `GroupCoordinator` [elected](consumer-groups/GroupCoordinator.md#onElection) to handle consumer groups that are "assigned" to this partition.

Default: `50`

Must be at least 1

Use `KafkaConfig.offsetsTopicPartitions` to access the current value.

Used when:

* `GroupCoordinator` is requested to [create an OffsetConfig](consumer-groups/GroupCoordinator.md#offsetConfig-KafkaConfig)
* `DefaultAutoTopicCreationManager` is requested to [creatableTopic](DefaultAutoTopicCreationManager.md#creatableTopic)
* `KafkaServer` is requested to [start up](broker/KafkaServer.md#startup) (and starts up the [GroupCoordinator](broker/KafkaServer.md#groupCoordinator))
* `BrokerMetadataPublisher` is requested to `initializeManagers` (and starts up the `GroupCoordinator`)

## <span id="offsetsTopicReplicationFactor"><span id="OffsetsTopicReplicationFactorProp"><span id="offsets.topic.replication.factor"> offsets.topic.replication.factor

## <span id="QuorumVotersProp"><span id="quorumVoters"> controller.quorum.voters { #controller.quorum.voters }

[controller.quorum.voters](kraft/RaftConfig.md#QUORUM_VOTERS_CONFIG)

## <span id="ProcessRolesProp"><span id="parseProcessRoles"><span id="processRoles"><span id="usesSelfManagedQuorum"> process.roles { #process.roles }

A comma-separated list of the roles that this Kafka server plays in a Kafka cluster:

Supported values:

* `broker`
* `controller`
* `broker,controller`

Default: (empty)

1. When empty, the process requires Zookeeper (runs with Zookeeper).
1. Only applicable for clusters in [KRaft (Kafka Raft) mode](kraft/index.md)
1. If used, [controller.quorum.voters](kraft/RaftConfig.md#QUORUM_VOTERS_CONFIG) must contain a parseable set of voters
1. [advertised.listeners](#AdvertisedListenersProp) config must not contain KRaft controller listeners from [controller.listener.names](#ControllerListenerNamesProp) when `process.roles` contains `broker` role because Kafka clients that send requests via advertised listeners do not send requests to KRaft controllers -- they only send requests to KRaft brokers
1. If `process.roles` contains `controller` role, the [node.id](KafkaConfig.md#NodeIdProp) must be included in the set of voters [controller.quorum.voters](kraft/RaftConfig.md#QUORUM_VOTERS_CONFIG)
1. If `process.roles` contains just the `broker` role, the [node.id](KafkaConfig.md#NodeIdProp) must not be included in the set of voters [controller.quorum.voters](kraft/RaftConfig.md#QUORUM_VOTERS_CONFIG)
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

## <span id="UncleanLeaderElectionEnableProp"><span id="unclean.leader.election.enable"> unclean.leader.election.enable

Enables replicas not in the ISR to be elected as leaders as a last resort, even though it is not guaranteed to have every committed message (and may even result in data loss).

It is to support use cases where uptime and availability are preferable over consistency and allow non-in-sync replicas to become partition leaders.

Default: `false` (disabled)

Unclean leader election is automatically enabled by the controller when this config is dynamically updated by using per-topic config override.

Use `KafkaConfig.uncleanLeaderElectionEnable` to access the current value.

Per-topic configuration: [unclean.leader.election.enable](TopicConfig.md#unclean.leader.election.enable)

Used when:

* `TopicConfigHandler` is requested to [processConfigChanges](dynamic-broker-configuration/TopicConfigHandler.md#processConfigChanges) (to [enableTopicUncleanLeaderElection](controller/KafkaController.md#enableTopicUncleanLeaderElection) on an active controller)

## Utilities

### <span id="dynamicConfig"> DynamicBrokerConfig

```scala
dynamicConfig: DynamicBrokerConfig
```

`KafkaConfig` initializes `dynamicConfig` when [created](#creating-instance) (based on the optional[dynamicConfigOverride](#dynamicConfigOverride)).

The `DynamicBrokerConfig` is used when:

* FIXME

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

## Creating Instance

`KafkaConfig` takes the following to be created:

* <span id="doLog"> `doLog` flag
* <span id="props"> Properties
* [DynamicBrokerConfig](#dynamicConfigOverride)

`KafkaConfig` is created when:

* `DynamicBrokerConfig` is requested to [initialize](dynamic-broker-configuration/DynamicBrokerConfig.md#initialize), [reloadUpdatedFilesWithoutConfigChange](dynamic-broker-configuration/DynamicBrokerConfig.md#reloadUpdatedFilesWithoutConfigChange), [processReconfiguration](dynamic-broker-configuration/DynamicBrokerConfig.md#processReconfiguration)
* `KafkaConfig` is requested to [fromProps](#fromProps), [apply](#apply)

### <span id="dynamicConfigOverride"> dynamicConfigOverride

`KafkaConfig` can be given a [DynamicBrokerConfig](dynamic-broker-configuration/DynamicBrokerConfig.md) when [created](#creating-instance).

!!! note
    `DynamicBrokerConfig` seems never be given.

`KafkaConfig` creates a new `DynamicBrokerConfig` for [dynamicConfig](#dynamicConfig) unless given.

## Creating KafkaConfig Instance

### <span id="fromProps"> fromProps

```scala
fromProps(
  props: Properties): KafkaConfig
fromProps(
  props: Properties,
  doLog: Boolean): KafkaConfig
fromProps(
  defaults: Properties,
  overrides: Properties): KafkaConfig
fromProps(
  defaults: Properties,
  overrides: Properties,
  doLog: Boolean): KafkaConfig
```

`fromProps`...FIXME

---

`fromProps` is used when:

* `Kafka` is requested to [build a Server](Kafka.md#buildServer)
* `AclAuthorizer` is requested to [configure](authorization/AclAuthorizer.md#configure)

### <span id="apply"> apply

```scala
apply(
  props: Map[_, _],
  doLog: Boolean = true): KafkaConfig
```

`apply`...FIXME

---

`apply` seems to be used for testing only.

## metadataLogDir { #metadataLogDir }

```scala
metadataLogDir: String
```

`metadataLogDir` is the value of [metadata.log.dir](#metadata.log.dir), if defined, or the first directory from [logDirs](#logDirs).

---

`metadataLogDir` is used when:

* `KafkaRaftManager` is [created](kraft/KafkaRaftManager.md#dataDirLock) and requested to [createDataDir](kraft/KafkaRaftManager.md#createDataDir)
* `KafkaRaftServer` is requested to [initializeLogDirs](kraft/KafkaRaftServer.md#initializeLogDirs)
* `StorageTool` is requested to [configToLogDirectories](tools/kafka-storage/StorageTool.md#configToLogDirectories)
