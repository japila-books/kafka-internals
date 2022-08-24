# TopicConfig

`TopicConfig` is configuration properties of Kafka topics. In other words, `TopicConfig` is a topic-specific configuration properties (while [KafkaConfig](KafkaConfig.md) is broker-wide).

!!! tip
    While reviewing the source code it can get tricky to find broker-wide properties (e.g. `log.cleanup.policy`). The reason is that broker-wide properties as split into `log.` prefix and a corresponding topic-specific property.

    A solution is to search for a topic-specific property removing `log.` prefix.

## <span id="CLEANUP_POLICY_CONFIG"><span id="cleanup.policy"> cleanup.policy

A comma-separated list of [cleanup policies](log-cleanup/index.md):

* `compact`
* `delete`

Default: `delete`

Broker-wide configuration: [log.cleanup.policy](KafkaConfig.md#LogCleanupPolicyProp)

Used when:

* `GroupCoordinator` is requested for the [offsetsTopicConfigs](consumer-groups/GroupCoordinator.md#offsetsTopicConfigs)
* `LogConfig` is requested to [compact](log/LogConfig.md#compact), [delete](log/LogConfig.md#delete), [TopicConfigSynonyms](log/LogConfig.md#TopicConfigSynonyms) and [extractLogConfigMap](log/LogConfig.md#extractLogConfigMap)
* `TopicBasedRemoteLogMetadataManager` is requested to `createRemoteLogMetadataTopicRequest`
* `TransactionStateManager` is requested for the [transactionTopicConfigs](transactions/TransactionStateManager.md#transactionTopicConfigs)
