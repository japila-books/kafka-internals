---
hide:
  - navigation
---

# Demo: Transactional Kafka Producer

This demo shows the internals of transactional [KafkaProducer](../clients/producer/KafkaProducer.md) that is a Kafka producer with [transaction.id](../clients/producer/ProducerConfig.md#TRANSACTIONAL_ID_CONFIG) defined.

## Create KafkaProducer

```text
import org.apache.kafka.clients.producer.KafkaProducer
import org.apache.kafka.clients.producer.ProducerConfig
import org.apache.kafka.common.serialization.StringSerializer

import java.util.Properties
val props = new Properties()
props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, classOf[StringSerializer].getName)
props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, classOf[StringSerializer].getName)
props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, ":9092")
// Define transaction.id
props.put(ProducerConfig.TRANSACTIONAL_ID_CONFIG, "my-custom-txnId")
val producer = new KafkaProducer[String, String](props)
```

### initTransactions

```text
producer.initTransactions
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
