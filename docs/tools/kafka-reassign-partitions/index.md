# kafka-reassign-partitions

`kafka-reassign-partitions` utility is used to move topic partitions between replicas (based on a [partition reassignment JSON file](ReassignPartitionsCommand.md#reassignment-json-file)).

`kafka-reassign-partitions` uses [ReassignPartitionsCommand](ReassignPartitionsCommand.md) for its execution.

## Options

``` console
$ ./bin/kafka-reassign-partitions.sh --help
This tool helps to move topic partitions between replicas.
Option                                  Description
------                                  -----------
--additional                            Execute this reassignment in addition
                                          to any other ongoing ones. This
                                          option can also be used to change
                                          the throttle of an ongoing
                                          reassignment.
...
```

## Demo

[Demo: kafka-reassign-partitions](demo.md)
