# Kafka Consumers

[KafkaConsumer](KafkaConsumer.md) uses [Fetcher](Fetcher.md) to fetch records from a Kafka cluster. One could say that `KafkaConsumer` is a developer-oriented interface to `Fetcher`.

`KafkaConsumer` is assigned a `IsolationLevel` based on [isolation.level](ConsumerConfig.md#ISOLATION_LEVEL_CONFIG) configuration property.
