# Serdes Utility

`Serdes` is a utility with the serializers and deserializers for many built-in types in Java and [allows defining new ones](#serdeFrom).

```text
import org.apache.kafka.common.serialization.Serdes

val longSerde = Serdes.Long
scala> :type longSerde
org.apache.kafka.common.serialization.Serde[Long]

scala> :type longSerde.serializer
org.apache.kafka.common.serialization.Serializer[Long]

scala> :type longSerde.deserializer
org.apache.kafka.common.serialization.Deserializer[Long]
```

## <span id="WrapperSerde"> WrapperSerde

`Serdes` defines a `static public class WrapperSerde<T>` that is a [Serde](Serde.md) from and to `T` values.

* `ShortSerde`
* `BytesSerde`
* `IntegerSerde`
* `ListSerde`
* `UUIDSerde`
* `FloatSerde`
* `FullTimeWindowedSerde` (Kafka Streams)
* `VoidSerde`
* `LongSerde`
* `DoubleSerde`
* `ByteArraySerde`
* `TimeWindowedSerde` (Kafka Streams)
* `SessionWindowedSerde` (Kafka Streams)
* `ByteBufferSerde`
* `StringSerde`

## <span id="serdeFrom"> serdeFrom

```java
Serde<T> serdeFrom(
  Class<T> type)
Serde<T> serdeFrom(
  Serializer<T> serializer,
  Deserializer<T> deserializer)
```

`serdeFrom` looks up the [Serde](Serde.md) for a given type (if supported) or creates a new one based on the given pair of `Serializer` and `Deserializer`.
