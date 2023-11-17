# Kafka Tiered Storage

In tiered storage approach, a Kafka cluster is configured with two tiers of storage: local and remote.

The local tier is the same as the current Kafka that uses the local disks on the Kafka brokers to store the log segments.
The new remote tier uses systems (e.g., HDFS, S3) to store the completed log segments.

Tiered storage is enabled using [remote.log.storage.system.enable](RemoteLogManagerConfig.md#remote.log.storage.system.enable) property.

Tiered storage is not supported with multiple [log directories](../KafkaConfig.md#logDirs).

## Learn More

1. [KIP-405: Kafka Tiered Storage]({{ kafka.wiki }}/KIP-405%3A+Kafka+Tiered+Storage)
