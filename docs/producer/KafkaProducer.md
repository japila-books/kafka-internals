# KafkaProducer

`KafkaProducer<K, V>` is a [Producer](Producer.md).

## Creating Instance

`KafkaProducer` takes the following to be created:

* <span id="config"><span id="producerConfig"> ProducerConfig
* <span id="keySerializer"> Key `Serializer<K>`
* <span id="valueSerializer"> Value `Serializer<V>`
* <span id="metadata"> ProducerMetadata
* <span id="kafkaClient"> KafkaClient
* <span id="interceptors"> `ProducerInterceptor<K, V>`s
* <span id="time"> Time

`KafkaProducer` is created when:

* FIXME

## <span id="abortTransaction"> abortTransaction

```scala
void abortTransaction()
```

`abortTransaction` prints out the following INFO message to the logs:

```text
Aborting incomplete transaction
```

`abortTransaction`...FIXME

`abortTransaction` is part of the [Producer](Producer.md#abortTransaction) abstraction.

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

## Logging

Enable `ALL` logging level for `org.apache.kafka.clients.producer.KafkaProducer` logger to see what happens inside.

Add the following line to `log4j.properties`:

```text
log4j.logger.org.apache.kafka.clients.producer.KafkaProducer=ALL
```

Refer to [Logging](logging.md).
