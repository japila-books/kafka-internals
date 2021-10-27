# AbstractCoordinator

`AbstractCoordinator` is an [abstraction](#contract) of [consumer group coordination manager](#implementations).

## Contract

### <span id="metadata"> metadata

```java
JoinGroupRequestData.JoinGroupRequestProtocolCollection metadata()
```

Used when:

* `AbstractCoordinator` is requested to [ensureActiveGroup](#ensureActiveGroup) (and [sendJoinGroupRequest](#sendJoinGroupRequest))

### <span id="onJoinComplete"> onJoinComplete

```java
void onJoinComplete(
  int generation,
  String memberId,
  String protocol,
  ByteBuffer memberAssignment)
```

Used when:

* `AbstractCoordinator` is requested to [ensureActiveGroup](#ensureActiveGroup) (and [joinGroupIfNeeded](#joinGroupIfNeeded))

### <span id="onJoinPrepare"> onJoinPrepare

```java
void onJoinPrepare(
  int generation,
  String memberId)
```

Used when:

* `AbstractCoordinator` is requested to [ensureActiveGroup](#ensureActiveGroup) (and [joinGroupIfNeeded](#joinGroupIfNeeded))

### <span id="performAssignment"> performAssignment

```java
Map<String, ByteBuffer> performAssignment(
  String leaderId,
  String protocol,
  List<JoinGroupResponseData.JoinGroupResponseMember> allMemberMetadata)
```

Used when:

* `AbstractCoordinator` is requested to [handle a JoinGroup response (having joined the group as the leader)](#onJoinLeader)

### <span id="protocolType"> protocolType

```java
String protocolType()
```

Used when:

* `AbstractCoordinator` is requested to [sendJoinGroupRequest](#sendJoinGroupRequest), [onJoinFollower](#onJoinFollower), [onJoinLeader](#onJoinLeader), [isProtocolTypeInconsistent](#isProtocolTypeInconsistent)

## Implementations

* [ConsumerCoordinator](ConsumerCoordinator.md)
* `WorkerCoordinator` (Kafka Connect)

## Handling JoinGroup Response

### <span id="onJoinLeader"> Group Leader

```java
RequestFuture<ByteBuffer> onJoinLeader(
  JoinGroupResponse joinResponse)
```

`onJoinLeader` [performAssignment](#performAssignment) (with the leader ID, the group protocol and the members).

`onJoinLeader` prints out the following DEBUG message to the logs:

```text
Sending leader SyncGroup to coordinator [coordinator] at generation [generation]: [SyncGroupRequest]
```

In the end, `onJoinLeader` sends a `SyncGroupRequest` to the coordinator.

`onJoinLeader` is used when:

* [JoinGroupResponseHandler](#JoinGroupResponseHandler) is requested to handle a `JoinGroup` response (after joining group successfully as the leader)

### <span id="onJoinFollower"> Group Follower

```java
RequestFuture<ByteBuffer> onJoinFollower()
```

`onJoinFollower`...FIXME

`onJoinFollower` is used when:

* [JoinGroupResponseHandler](#JoinGroupResponseHandler) is requested to handle a `JoinGroup` response (after joining group successfully as a follower)

## <span id="ensureActiveGroup"> ensureActiveGroup

```java
void ensureActiveGroup()
boolean ensureActiveGroup(
  Timer timer)
```

`ensureActiveGroup` [ensureCoordinatorReady](#ensureCoordinatorReady) (and returns `false` if not).

`ensureActiveGroup`...FIXME

`ensureActiveGroup` is used when:

* `ConsumerCoordinator` is requested to [poll](ConsumerCoordinator.md#poll)
* `WorkerCoordinator` (Kafka Connect) is requested to `poll`

### <span id="joinGroupIfNeeded"> joinGroupIfNeeded

```java
boolean joinGroupIfNeeded(
  Timer timer)
```

`joinGroupIfNeeded`...FIXME

### <span id="initiateJoinGroup"> initiateJoinGroup

```java
RequestFuture<ByteBuffer> initiateJoinGroup()
```

`initiateJoinGroup`...FIXME

### <span id="sendJoinGroupRequest"> sendJoinGroupRequest

```java
RequestFuture<ByteBuffer> sendJoinGroupRequest()
```

`sendJoinGroupRequest`...FIXME

## <span id="ensureCoordinatorReady"> Waiting for Consumer Group Coordinator Known and Ready

```java
boolean ensureCoordinatorReady(
  Timer timer)
```

`ensureCoordinatorReady` returns `true` immediately when the [consumer group coordinator is known and available](#coordinatorUnknown).

Otherwise, `ensureCoordinatorReady` keeps [looking up the group coordinator](#lookupCoordinator) (by sending `FindCoordinator` requests to the least loaded broker) until the coordinator is available or the timer timed out.

In the end, `ensureCoordinatorReady` returns whether the [coordinator is known and available](#coordinatorUnknown) or not.

`ensureCoordinatorReady` is used when:

* `AbstractCoordinator` is requested to [ensureActiveGroup](#ensureActiveGroup) (and [joinGroupIfNeeded](#joinGroupIfNeeded))
* `ConsumerCoordinator` is requested to [poll](ConsumerCoordinator.md#poll), [fetchCommittedOffsets](ConsumerCoordinator.md#fetchCommittedOffsets), [close](ConsumerCoordinator.md#close), [commitOffsetsSync](ConsumerCoordinator.md#commitOffsetsSync)

## <span id="JoinGroupResponseHandler"> JoinGroupResponseHandler

`JoinGroupResponseHandler` is a `CoordinatorResponseHandler` to handle responses from the [group coordinator](#coordinator) after [sendJoinGroupRequest](#sendJoinGroupRequest).
