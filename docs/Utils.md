# Utils

## <span id="murmur2"> murmur2

```java
int murmur2(
  byte[] data)
```

`murmur2` generates a 32-bit murmur2 hash for the given byte array.

`murmur2` is used when:

* `DefaultPartitioner` is requested to [compute a partition for a record](producer/DefaultPartitioner.md#partition)

### <span id="murmur2-demo"> Demo

```text
import org.apache.kafka.common.utils.Utils

val keyBytes = "hello".getBytes
val hash = Utils.murmur2(keyBytes)

println(hash)
```

## <span id="toPositive"> toPositive

```java
int toPositive(
  int number)
```

`toPositive` converts a number to a positive value.
