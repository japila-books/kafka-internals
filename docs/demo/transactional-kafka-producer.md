---
hide:
  - navigation
---

# Demo: Transactional Kafka Producer

This demo shows the internals of transactional [KafkaProducer](../clients/producer/KafkaProducer.md) that is a Kafka producer with [transaction.id](../clients/producer/ProducerConfig.md#TRANSACTIONAL_ID_CONFIG) defined.

## KafkaProducer

### Start Up

Use `sbt console` for interactive environment (or IntelliJ IDEA).

```text
import org.apache.kafka.clients.producer.KafkaProducer
import org.apache.kafka.clients.producer.ProducerConfig
import org.apache.kafka.common.serialization.StringSerializer

import java.util.Properties
val props = new Properties()
props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, classOf[StringSerializer].getName)
props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, classOf[StringSerializer].getName)
props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, ":9092")
props.put(ProducerConfig.CLIENT_ID_CONFIG, "txn-demo")
// Define transaction.id
val transactionalId = "my-custom-txnId"
props.put(ProducerConfig.TRANSACTIONAL_ID_CONFIG, transactionalId)
val producer = new KafkaProducer[String, String](props)
```

### Initialize Transactions

```text
producer.initTransactions
```

Once initialized, the transactional producer must not be initialized again.

```text
scala> producer.initTransactions
org.apache.kafka.common.KafkaException: TransactionalId my-custom-txnId: Invalid transition attempted from state READY to state INITIALIZING
  at org.apache.kafka.clients.producer.internals.TransactionManager.transitionTo(TransactionManager.java:1078)
  at org.apache.kafka.clients.producer.internals.TransactionManager.transitionTo(TransactionManager.java:1071)
  at org.apache.kafka.clients.producer.internals.TransactionManager.lambda$initializeTransactions$1(TransactionManager.java:337)
  at org.apache.kafka.clients.producer.internals.TransactionManager.handleCachedTransactionRequestResult(TransactionManager.java:1200)
  at org.apache.kafka.clients.producer.internals.TransactionManager.initializeTransactions(TransactionManager.java:334)
  at org.apache.kafka.clients.producer.internals.TransactionManager.initializeTransactions(TransactionManager.java:329)
  at org.apache.kafka.clients.producer.KafkaProducer.initTransactions(KafkaProducer.java:596)
  ... 31 elided
```

### Start Transaction

Next up is starting a transaction using [KafkaProducer.beginTransaction](../clients/producer/KafkaProducer.md#beginTransaction)

```text
producer.beginTransaction
```

### Transactional sends

```text
import org.apache.kafka.clients.producer.ProducerRecord
val topic = "txn-demo"
val record = new ProducerRecord[String, String](topic, "Hello from transactional producer")
producer.send(record)
```

```text
producer.send(
  new ProducerRecord[String, String](topic, "Another hello from txn producer"))
```

### Logs

#### Kafka Producer

You should see the following INFO messages in the logs of the Kafka producer:

```text
INFO [Producer clientId=producer-my-custom-txnId, transactionalId=my-custom-txnId] Invoking InitProducerId for the first time in order to acquire a producer ID (org.apache.kafka.clients.producer.internals.TransactionManager)
INFO [Producer clientId=producer-my-custom-txnId, transactionalId=my-custom-txnId] Discovered transaction coordinator localhost:9092 (id: 0 rack: null) (org.apache.kafka.clients.producer.internals.TransactionManager)
INFO [Producer clientId=producer-my-custom-txnId, transactionalId=my-custom-txnId] ProducerId set to 0 with epoch 0 (org.apache.kafka.clients.producer.internals.TransactionManager)
```

#### Kafka Cluster

You should see the following INFO message in the logs of a Kafka cluster:

```text
INFO [TransactionCoordinator id=0] Initialized transactionalId my-custom-txnId with producerId 0 and producer epoch 0 on partition __transaction_state-20 (kafka.coordinator.transaction.TransactionCoordinator)
```

The calculation to [determine the transactional partition](../transactions/TransactionStateManager.md#partitionFor) (`__transaction_state-20`) is as follows:

```scala
Math.abs(transactionalId.hashCode) % 50
```

## Start Up Consumer

### kcat

```text
kcat -C -b localhost -t txn-demo
```

You should see no records produced yet (since the transaction has not been committed yet).

### kafka-console-consumer

```text
./bin/kafka-console-consumer.sh \
  --bootstrap-server :9092 \
  --topic txn-demo \
  --from-beginning
```

Unlike `kcat`, `kafka-console-consumer` uses [read_uncommitted](../clients/consumer/ConsumerConfig.md#isolation.level) isolation level and so there should be records printed out to the console.

Use `--isolation-level` option to set [isolation.level](../clients/consumer/ConsumerConfig.md#isolation.level) configuration property.

## Commit Transaction

Let's commit the transaction using [KafkaProducer.commitTransaction](../clients/producer/KafkaProducer.md#commitTransaction).

```text
producer.commitTransaction
```

Immediately after committing the transaction you should see the record printed out by the Kafka consumer.
