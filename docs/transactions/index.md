# Transactions

Apache Kafka supports transactional record delivery (and consumption if in consumer-process-produce processing mode).

Every Kafka broker runs a [TransactionCoordinator](TransactionCoordinator.md) to manage (_coordinate_) transactions.

## Transactional Producer

A [KafkaProducer](../clients/producer/KafkaProducer.md) is [transactional](../clients/producer/TransactionManager.md#isTransactional) when [transactional.id](../clients/producer/ProducerConfig.md#transactional.id) configuration property is specified.

Any [record sending](../clients/producer/KafkaProducer.md#send) has to be after [KafkaProducer.initTransactions](../clients/producer/KafkaProducer.md#initTransactions) followed by [KafkaProducer.beginTransaction](../clients/producer/KafkaProducer.md#beginTransaction). Otherwise, the underlying [TransactionManager](../clients/producer/TransactionManager.md) is going to be in a wrong [state](../clients/producer/TransactionManager.md#states) (that will inevitably lead to exceptions).

## Demo

[Demo: Transactional Kafka Producer](../demo/transactional-kafka-producer.md)

## <span id="TransactionConfig"> Transactional Configuration Properties

[TransactionConfig](TransactionConfig.md)

## <span id="TRANSACTION_STATE_TOPIC_NAME"> Transaction Topic

Kafka brokers use `__transaction_state` internal topic for managing transactions (as [records](TransactionStateManager.md#appendTransactionToLog)).

`__transaction_state` is [auto-created](../DefaultAutoTopicCreationManager.md#creatableTopic) at the first transaction.

The number of partitions is configured using [transaction.state.log.num.partitions](../KafkaConfig.md#transaction.state.log.num.partitions) configuration property.

A transaction (record) is assigned a partition (_txn topic partition_) based on the [absolute hash code](TransactionStateManager.md#partitionFor) of the [transactional.id](../clients/producer/ProducerConfig.md#transactional.id).

## Learning Resources

* [Transactions in Apache Kafka](https://www.confluent.io/blog/transactions-apache-kafka/) by Confluent
