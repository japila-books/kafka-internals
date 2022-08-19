# kafka-replica-verification

`kafka-replica-verification` utility is used to verify replica consistency (i.e., validate that all replicas for a set of topics have the same data).

## Options

``` console
$ ./bin/kafka-replica-verification.sh --help
Validate that all replicas for a set of topics have the same data.
Option                                  Description
------                                  -----------
--broker-list <String: hostname:        REQUIRED: The list of hostname and
  port,...,hostname:port>                 port of the server to connect to.
--fetch-size <Integer: bytes>           The fetch size of each request.
                                          (default: 1048576)
--help                                  Print usage information.
--max-wait-ms <Integer: ms>             The max amount of time each fetch
                                          request waits. (default: 1000)
--report-interval-ms <Long: ms>         The reporting interval. (default:
                                          30000)
--time <Long: timestamp/-1(latest)/-2   Timestamp for getting the initial
  (earliest)>                             offsets. (default: -1)
--topic-white-list <String: Java regex  DEPRECATED use --topics-include
  (String)>                               instead; ignored if --topics-include
                                          specified. List of topics to verify
                                          replica consistency. Defaults to '.
                                          *' (all topics) (default: .*)
--topics-include <String: Java regex    List of topics to verify replica
  (String)>                               consistency. Defaults to '.*' (all
                                          topics) (default: .*)
--version                               Print version information and exit.
```

## Demo

``` console
$ ./bin/kafka-replica-verification.sh --broker-list :9092
verification process is started.
```
