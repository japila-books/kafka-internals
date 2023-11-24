# RaftConfig

`RaftConfig` is the configuration of [KafkaRaftManager](KafkaRaftManager.md#raftConfig) in [KRaft mode](index.md).

## Creating Instance

`RaftConfig` takes the following to be created:

* <span id="voterConnections"> [controller.quorum.voters](#QUORUM_VOTERS_CONFIG)
* <span id="requestTimeoutMs"> `requestTimeoutMs`
* <span id="retryBackoffMs"> `retryBackoffMs`
* <span id="electionTimeoutMs"> `electionTimeoutMs`
* <span id="electionBackoffMaxMs"> `electionBackoffMaxMs`
* <span id="fetchTimeoutMs"> `fetchTimeoutMs`
* <span id="appendLingerMs"> `appendLingerMs`

`RaftConfig` is created when:

* `KafkaRaftManager` is [created](KafkaRaftManager.md#raftConfig)

## <span id="QUORUM_VOTERS_CONFIG"> controller.quorum.voters { #controller.quorum.voters }

A comma-separated list of `{id}@{host}:{port}` with ids and endpoints of quorum voters (e.g., `1@localhost:9092,2@localhost:9093,3@localhost:9094`)

Default: (empty)

Available as [KafkaConfig.quorumVoters](../KafkaConfig.md#quorumVoters)

Used when:

* `RaftConfig` is requested to [parseVoterConnections](#parseVoterConnections)
* `KafkaConfig` is requested for [QuorumVotersProp](../KafkaConfig.md#QuorumVotersProp) and [quorumVoters](../KafkaConfig.md#quorumVoters)

### parseVoterConnections { #parseVoterConnections }

```java
Map<Integer, AddressSpec> parseVoterConnections(
  List<String> voterEntries)
```

`parseVoterConnections`...FIXME

---

`parseVoterConnections` is used when:

* `RaftConfig` is [created](#voterConnections) and requested to [quorumVoterStringsToNodes](#quorumVoterStringsToNodes)
* `KafkaConfig` is requested to [validateValues](../KafkaConfig.md#validateValues)
* `KafkaRaftServer` is [created](KafkaRaftServer.md#controllerQuorumVotersFuture)
* `KafkaServer` is requested to [start up](../broker/KafkaServer.md#startup)
