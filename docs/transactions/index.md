# Transactions

<span id="TRANSACTION_STATE_TOPIC_NAME">
Kafka brokers use `__transaction_state` topic for [storing transaction records](../TransactionStateManager.md#appendTransactionToLog).

The number of partitions is configured using...FIXME

A transaction is assigned a partition (_txn topic partition_) based on the [absolute hash code](../TransactionStateManager.md#partitionFor).

## Learning Resources

* [Transactions in Apache Kafka](https://www.confluent.io/blog/transactions-apache-kafka/) by Confluent
