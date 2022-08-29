# kafka-get-offsets

`kafka-get-offsets` utility is an interactive shell for getting topic-partition offsets.

```console
$ ./bin/kafka-get-offsets.sh
An interactive shell for getting topic-partition offsets.
```

`kafka-get-offsets` uses [GetOffsetShell](GetOffsetShell.md) for execution.

```console
$ ./bin/kafka-topics.sh \
    --bootstrap-server :9092 \
    --create \
    --if-not-exists \
    --topic demo-get-offsets \
    --partitions 3
```

```console
$ ./bin/kafka-get-offsets.sh \
    --bootstrap-server :9092
demo-get-offsets:0:0
demo-get-offsets:1:0
demo-get-offsets:2:0
```

```console
$ echo zero | kcat -P -b :9092 -t demo-get-offsets -p 0
```

```console
$ ./bin/kafka-get-offsets.sh \
    --bootstrap-server :9092 \
    --topic demo-get-offsets
demo-get-offsets:0:1
demo-get-offsets:1:0
demo-get-offsets:2:0
```

```console
$ ./bin/kafka-get-offsets.sh \
    --bootstrap-server :9092 \
    --topic-partitions demo-get-offsets:0,demo-get-offsets:2
demo-get-offsets:0:1
demo-get-offsets:2:0
```

## Options

### exclude-internal-topics

### partitions

### time

### topic

### topic-partitions
