# Kafka Producers

[KafkaProducer](KafkaProducer.md) uses [RecordAccumulator](KafkaProducer.md#accumulator) with records to be sent out.

`KafkaProducer` groups together records that arrive in-between request transmissions into a single batched request.
Normally this occurs only under load when records arrive faster than they can be sent out.
However in some circumstances the client may want to reduce the number of requests even under moderate load using [linger.ms](ProducerConfig.md#linger.ms) configuration property.

`KafkaProducer` uses [Sender](Sender.md) to send records to a Kafka cluster.

`KafkaProducer` can be [transactional or idempotent](KafkaProducer.md#configureTransactionState) (and associated with a [TransactionManager](TransactionManager.md)).

## Demo

```text
// Necessary imports
import org.apache.kafka.clients.producer.KafkaProducer
import org.apache.kafka.clients.producer.ProducerConfig
import org.apache.kafka.common.serialization.StringSerializer

// Creating a KafkaProducer
import java.util.Properties
val props = new Properties()
props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, classOf[StringSerializer].getName)
props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, classOf[StringSerializer].getName)
props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, ":9092")
val producer = new KafkaProducer[String, String](props)

// Creating a record to be sent
import org.apache.kafka.clients.producer.ProducerRecord
val r = new ProducerRecord[String, String]("0", "this is a message")

// Sending the record (with no Callback)
import java.util.concurrent.Future
import org.apache.kafka.clients.producer.RecordMetadata
val metadataF: Future[RecordMetadata] = producer.send(r)
```
