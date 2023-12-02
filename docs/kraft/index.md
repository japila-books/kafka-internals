# Kafka Raft (KRaft)

**Kafka Raft (KRaft)** is a [Raft](https://en.wikipedia.org/wiki/Raft_(algorithm))-based distributed consensus algorithm in Apache Kafka to manage cluster metadata.

There are two types of servers in KRaft mode:

* [broker](#brokers)
* [controller](#controllers)

A single Kafka server can be one or both (`broker,controller`) at the same time based on [process.roles](../KafkaConfig.md#process.roles) configuration property.

A Kafka server is [KafkaRaftServer](KafkaRaftServer.md) in [KRaft mode](index.md).
When created, [KafkaRaftServer](KafkaRaftServer.md) creates a [SharedServer](SharedServer.md) that is then used to create a [BrokerServer](BrokerServer.md) or a [ControllerServer](ControllerServer.md) or both (based on [process.roles](../KafkaConfig.md#process.roles) configuration property).

In KRaft mode, [controllers](#controllers) store cluster metadata in the internal [__cluster_metadata](#metadata-topic) topic.

KRaft allows running a Kafka cluster without Apache ZooKeeper in so-called **Kafka Raft metadata mode** (_KRaft mode_).

With KRaft, a Kafka cluster uses its own Kafka infrastructure for metadata (with no need for a separate system, e.g. Zookeeper).

In KRaft mode, the storage log directories on a node must be formatted using [kafka-storage](../tools/kafka-storage/index.md) utility.
This requirement prevents system administrators from accidentally enabling KRaft mode by simply making a configuration change.
Requiring formatting also prevents mistakes, since Kafka no longer has to guess if an empty storage directory is a newly directory or one where a system error prevented any data from showing up.

KRaft was proposed as [KIP-500]({{ kafka.wiki }}/KIP-500%3A+Replace+ZooKeeper+with+a+Self-Managed+Metadata+Quorum) and went live in [Kafka 2.8.0]({{ kafka.jira }}/KAFKA-9119).

KRaft is production ready since [Apache Kafka 3.3.1]({{ kafka.wiki }}/KIP-833%3A+Mark+KRaft+as+Production+Ready).

## Metadata Topic

[Controllers](#controllers) use an internal `__cluster_metadata` topic to store the cluster metadata.

This topic contains a single partition.

Metadata changes are persisted to the `__cluster_metadata` log before applying them to the other nodes in the cluster.
This means waiting for the metadata log's last stable offset to advance to the offset of the change.

Changes that we haven't yet persisted are referred to as "uncommitted."  The active controller may have several of these uncommitted changes in flight at any given time.  In essence, the controller's in-memory state is always a little bit in the future compared to the current state.  This allows the active controller to continue doing things while it waits for the previous changes to be committed to the Raft log.

## Node IDs

Node IDs must be set in the configuration file using `node.id` configuration.
No automatic node ID assignment is available.

In a co-located configuration, a single process may take both the `controller` and `broker` roles.
The node ID for both of these roles is defined by `node.id`.
However, this is mainly for configuration convenience.
Semantically, we view the co-located process as representing two distinct nodes.
Each node has its own listeners and its own set of APIs which it exposes.
The APIs exposed by a controller node will not be the same as those exposed by a broker node.

## Controllers

[KRaft Controllers](ControllerServer.md) are responsible for storing the metadata of the cluster in the metadata log.

Controllers participate in the [metadata quorum](#metadata-quorum).

In Kraft mode, the active controller is selected among a potentially smaller pool of nodes specifically configured to act as controllers.  Typically three or five nodes in a cluster will be selected to be controllers.

The leader of the controller quorum will be the active controller.
The followers will function as hot standbys, ready to take over when the active leader fails or resigns.
The cluster metadata is stored in memory on all of the controllers.

The active controller makes changes to the metadata by appending records to the log. Each record has a `null` key with some value.

System administrators will be able to choose whether to run separate controller nodes, or whether to run controller nodes which are co-located with broker nodes.
Kafka supports running a controller in the same JVM as a broker, in order to save memory and enable single-process test deployments.

The addresses and ports of the controller nodes must be configured on each broker, so that the broker can contact the controller quorum when starting up.
As long as at least one of the provided controller addresses is valid, the broker will be able to learn about the current metadata quorum and start up.

Controllers listen on a separate endpoint from brokers.
The endpoints should be firewalled off from clients to prevent clients from disrupting the cluster by flooding the controller ports with requests.

Controllers do not appear in the MetadataResponses given to clients.

Controllers do not host topics.

## Brokers

[KRaft Brokers](BrokerServer.md)

Brokers register with the active controller using a `BrokerRegistrationRequest`.
The active controller assigns the broker a new broker epoch, based on the next available offset in the log.
The new epoch is guaranteed to be higher than any previous epoch that has been used for the given broker id.

In its periodic heartbeats, the broker asks the controller if it can transition into the controlled shutdown state.
It does this by setting the WantShutDown boolean.
This motivates the controller to move all of the leaders off of that broker.
Once they are all moved, the controller responds to the heartbeat with `ShouldShutDown = true`.
At that point, the broker knows it's safe to begin the shutdown process proper.

The brokers are always fetching new metadata from the controller.

## meta.properties

When a storage directory is in use by a cluster running in KRaft mode, it will have a new version of the `meta.properties` file.
The version is `1`.
`meta.properties` file is a plain text file where each line has the format `key=value`.

The file contains `node.id` of the owner node.

The process will raise an error at startup if either the `meta.properties` file does not exist or if the `node.id` does not match what the value from the configuration file.

## Metrics

Full Name | Description
----------|------------
`kafka.controller:type=KafkaController,name=MetadataLag` | The offset delta between the latest metadata record this controller has replayed and the last stable offset of the metadata topic.
`kafka.controller:type=KafkaServer,name=MetadataLag` | The offset delta between the latest metadata record this broker has replayed and the last stable offset of the metadata topic.
`kafka.controller:type=KafkaController,name=MetadataCommitLatencyMs` | The latency of committing a message to the metadata topic.  Relevant on the active controller.
`kafka.controller:type=KafkaController,name=MetadataCommitRatePerSec` | The number of metadata messages per second committed to the metadata topic.
`kafka.controller:type=KafkaController,name=MetadataSnapshotOffsetLag` | The offset delta between the latest stable offset of the metadata topic and the offset of the last snapshot (or the last stable offset itself, if there are no snapshots)

## Raft-Based Metadata Quorum { #metadata-quorum }

[controller.quorum.voters](../KafkaConfig.md#quorumVoters)

It is possible to reconfigure the metadata quorum over time. For example, if we start with a metadata quorum of host1, host2, host3, we could replace host3 with host4 without disrupting any of the brokers. Then we could roll the brokers to apply the new metadata quorum bootstrap configuration of host1, host2, host4 on each one.

## Snapshots

Periodically, controllers consolidate all the metadata deltas into a snapshot.

Like the metadata log, the snapshot is made up of records.  However, unlike the log, in which there may be multiple records describing a single entity, the snapshot will only contain the minimum number of records needed to describe all the entities.

Snapshots are local to each replica.
Any snapshot must be usable as a starting point for loading the entire state of metadata.
In other words, a new controller node must be able to load the a snapshot, and then apply all the edits which follow it, and come up-to-date.

## KRaft Metadata Transactions

[KIP-868 Metadata Transactions]({{ kafka.wiki }}/KIP-868+Metadata+Transactions)

## Learn More

* [KIP-500: Replace ZooKeeper with a Self-Managed Metadata Quorum]({{ kafka.wiki }}/KIP-500%3A+Replace+ZooKeeper+with+a+Self-Managed+Metadata+Quorum)
* [KIP-631: The Quorum-based Kafka Controller]({{ kafka.wiki }}/KIP-631%3A+The+Quorum-based+Kafka+Controller)
* [KRaft Overview](https://docs.confluent.io/platform/current/kafka-metadata/kraft.html)
* [Raft (algorithm)](https://en.wikipedia.org/wiki/Raft_(algorithm)) on Wikipedia
