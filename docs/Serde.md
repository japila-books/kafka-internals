# Serde

`Serde` is an [abstraction](#contract) of [wrappers](#implementations) with [Serializer](#serializer)s and [Deserializer](#deserializer)s.

!!! note
    `Serde` seems to be of more use in libraries like Kafka Streams or Kafka Connect (that in the Kafka Core).

## Contract

### <span id="configure"> configure

```java
void configure(
  Map<String, ?> configs,
  boolean isKey)
```

### <span id="deserializer"> deserializer

```java
Deserializer<T> deserializer()
```

### <span id="serializer"> serializer

```java
Serializer<T> serializer()
```

## Implementations

* [WrapperSerde](Serdes.md#WrapperSerde)
