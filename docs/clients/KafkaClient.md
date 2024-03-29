# KafkaClient

`KafkaClient` is an [interface](#contract) to [NetworkClient](NetworkClient.md).

## Contract

### <span id="inFlightRequestCount"> inFlightRequestCount

```java
int inFlightRequestCount()
int inFlightRequestCount(
  String nodeId)
```

Used when:

* `ConsumerNetworkClient` is requested to [pendingRequestCount](consumer/ConsumerNetworkClient.md#pendingRequestCount) and [poll](consumer/ConsumerNetworkClient.md#poll)
* `Sender` is requested to [run](producer/Sender.md#run)
* `SenderMetrics` is requested for `requests-in-flight` performance metric

### <span id="leastLoadedNode"> leastLoadedNode

```java
Node leastLoadedNode(
  long now)
```

Used when:

* `ConsumerNetworkClient` is requested for the [leastLoadedNode](consumer/ConsumerNetworkClient.md#leastLoadedNode)
* `DefaultMetadataUpdater` is requested to `maybeUpdate`
* `KafkaAdminClient` is used
* `Sender` is requested for the [maybeSendAndPollTransactionalRequest](producer/Sender.md#maybeSendAndPollTransactionalRequest)

### <span id="newClientRequest"> newClientRequest

```java
ClientRequest newClientRequest(
  String nodeId,
  AbstractRequest.Builder<?> requestBuilder,
  long createdTimeMs,
  boolean expectResponse)
ClientRequest newClientRequest(
  String nodeId,
  AbstractRequest.Builder<?> requestBuilder,
  long createdTimeMs,
  boolean expectResponse,
  int requestTimeoutMs,
  RequestCompletionHandler callback)
```

Used when:

* `AdminClientRunnable` is requested to `sendEligibleCalls`
* `ConsumerNetworkClient` is requested to [send](consumer/ConsumerNetworkClient.md#send)
* `NetworkClient` is requested to [newClientRequest](NetworkClient.md#newClientRequest), [sendInternalMetadataRequest](NetworkClient.md#sendInternalMetadataRequest) and [handleInitiateApiVersionRequests](NetworkClient.md#handleInitiateApiVersionRequests)
* `RequestSendThread` is requested to `doWork`
* `Sender` is requested to [run](producer/Sender.md#run)
* `KafkaServer` is requested to `controlledShutdown`
* `ReplicaFetcherBlockingSend` is requested to `sendRequest`

### <span id="poll"> poll

```java
List<ClientResponse> poll(
  long timeout,
  long now)
```

Used when:

* FIXME

### <span id="pollDelayMs"> pollDelayMs

```java
long pollDelayMs(
  Node node,
  long now)
```

Used when:

* FIXME

### <span id="ready"> Is Node Ready and Connected

```java
boolean ready(
  Node node,
  long now);
```

Used when:

* `AdminClientRunnable` is requested to [sendEligibleCalls](admin/AdminClientRunnable.md#sendEligibleCalls)
* `ConsumerNetworkClient` is requested to [tryConnect](consumer/ConsumerNetworkClient.md#tryConnect) and [trySend](consumer/ConsumerNetworkClient.md#trySend)
* `InterBrokerSendThread` is requested to [sendRequests](../InterBrokerSendThread.md#sendRequests)
* `NetworkClientUtils` is requested to [awaitReady](NetworkClientUtils.md#awaitReady)
* `Sender` is requested to [sendProducerData](producer/Sender.md#sendProducerData)

### <span id="send"> send

```java
void send(
  ClientRequest request,
  long now)
```

Used when:

* FIXME

### <span id="wakeup"> wakeup

```java
void wakeup()
```

Used when:

* FIXME

## Implementations

* [NetworkClient](NetworkClient.md)

## <span id="Closeable"> Closeable

`KafkaClient` is a `Closeable` ([Java]({{ java.api }}/java/io/Closeable.html)).
