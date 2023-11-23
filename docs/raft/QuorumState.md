# QuorumState

`QuorumState` is used by [KafkaRaftClient](KafkaRaftClient.md#quorum) to...FIXME

## Creating Instance

`QuorumState` takes the following to be created:

* <span id="localId"> Local ID
* [Voters](#voters)
* <span id="electionTimeoutMs"> [controller.quorum.election.timeout.ms](RaftConfig.md#controller.quorum.election.timeout.ms)
* <span id="fetchTimeoutMs"> [controller.quorum.fetch.timeout.ms](RaftConfig.md#controller.quorum.fetch.timeout.ms)
* <span id="store"> `QuorumStateStore`
* <span id="time"> `Time`
* <span id="logContext"> `LogContext`
* <span id="random"> `Random`

`QuorumState` is created when:

* `KafkaRaftClient` is [created](KafkaRaftClient.md#quorum)

### voters

```java
Set<Integer> voters
```

`QuorumState` is given quorum voters (their IDs) when [created](#creating-instance) based on [controller.quorum.voters](RaftConfig.md#controller.quorum.voters) configuration property.
