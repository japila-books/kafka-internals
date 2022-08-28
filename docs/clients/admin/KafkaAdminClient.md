# KafkaAdminClient

`KafkaAdminClient` is a `AdminClient` that is used in [Kafka administration utilities](../../tools/index.md).

## Creating Instance

`KafkaAdminClient` takes the following to be created:

* <span id="config"> `AdminClientConfig`
* <span id="clientId"> Client ID
* <span id="time"> Time
* <span id="metadataManager"> `AdminMetadataManager`
* <span id="metrics"> [Metrics](../../metrics/Metrics.md)
* <span id="client"> [KafkaClient](../KafkaClient.md)
* <span id="timeoutProcessorFactory"> `TimeoutProcessorFactory`
* <span id="logContext"> `LogContext`

`KafkaAdminClient` is created using [createInternal](#createInternal).

## <span id="createInternal"> createInternal

```java
KafkaAdminClient createInternal(
  AdminClientConfig config,
  AdminMetadataManager metadataManager,
  KafkaClient client,
  Time time)
KafkaAdminClient createInternal(
  AdminClientConfig config,
  TimeoutProcessorFactory timeoutProcessorFactory) // (1)!
KafkaAdminClient createInternal(
  AdminClientConfig config,
  TimeoutProcessorFactory timeoutProcessorFactory,
  HostResolver hostResolver)
```

1. Uses an undefined `HostResolver`

`createInternal`...FIXME

---

`createInternal` is used when:

* `Admin` is requested to [create](Admin.md#create)

## Logging

Enable `ALL` logging level for `org.apache.kafka.clients.admin.KafkaAdminClient` logger to see what happens inside.

Add the following line to `config/log4j.properties`:

```text
log4j.logger.org.apache.kafka.clients.admin.KafkaAdminClient=ALL
```

Refer to [Logging](../../logging.md).

## Review Me

## <span id="electPreferredLeaders"> Triggerring Preferred Replica Leader Election

```java
ElectPreferredLeadersResult electPreferredLeaders(
  Collection<TopicPartition> partitions,
  ElectPreferredLeadersOptions options)
```

NOTE: `electPreferredLeaders` is part of the <<kafka-clients-admin-AdminClient.adoc#electPreferredLeaders, AdminClient Contract>> to trigger <<kafka-feature-preferred-replica-leader-election.adoc#, preferred replica leader election>>.

`electPreferredLeaders` creates a *electPreferredLeaders* call that simply uses <<kafka-common-requests-ElectPreferredLeadersRequest.adoc#Builder, ElectPreferredLeadersRequest.Builder>> to <<kafka-common-requests-ElectPreferredLeadersRequest.adoc#toRequestData, serialize partitions into a ElectPreferredLeadersRequest>> and, when a response comes in, requests the `ElectPreferredLeadersRequest` to <<kafka-common-requests-ElectPreferredLeadersRequest.adoc#fromResponseData, deserialize it>>.

In the end, `electPreferredLeaders` requests the <<runnable, AdminClientRunnable>> to <<kafka-clients-admin-KafkaAdminClient-AdminClientRunnable.adoc#call, enqueue>> the `electPreferredLeaders` call.
