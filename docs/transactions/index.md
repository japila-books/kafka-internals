# Transactions

Apache Kafka supports transactional record delivery.

Every Kafka broker runs a [TransactionCoordinator](TransactionCoordinator.md) to manage (_coordinate_) transactions.

## <span id="TransactionConfig"> Transactional Configuration Properties

[TransactionConfig](TransactionConfig.md)

## <span id="TRANSACTION_STATE_TOPIC_NAME"> Transaction Topic

Kafka brokers use `__transaction_state` internal topic for managing transactions (as [records](TransactionStateManager.md#appendTransactionToLog)).

`__transaction_state` is [auto-created](../DefaultAutoTopicCreationManager.md#creatableTopic) at the first transaction.

The number of partitions is configured using [transaction.state.log.num.partitions](../KafkaConfig.md#transaction.state.log.num.partitions) configuration property.

A transaction (record) is assigned a partition (_txn topic partition_) based on the [absolute hash code](TransactionStateManager.md#partitionFor) of the [transactional.id](../clients/producer/ProducerConfig.md#transactional.id).

## Learning Resources

* [Transactions in Apache Kafka](https://www.confluent.io/blog/transactions-apache-kafka/) by Confluent
