# DefaultPartitioner

`DefaultPartitioner` is a [Partitioner](Partitioner.md).

## Demo

```text
import org.apache.kafka.clients.producer.internals.DefaultPartitioner
val partitioner = new DefaultPartitioner

val keyBytes = "hello".getBytes
val numPartitions = 3

val p = partitioner.partition(null, null, keyBytes, null, null, null, numPartitions)

println(p)
```

The following snippet should generate the same partition value (since it is exactly how `DefaultPartitioner` does it).

```text
import org.apache.kafka.common.utils.Utils

val keyBytes = "hello".getBytes
val numPartitions = 3

val p = Utils.toPositive(Utils.murmur2(keyBytes)) % numPartitions

println(p)
```
