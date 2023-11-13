# kafka-storage Utility

`kafka-storage` script is used to...FIXME...when setting up a Kafka cluster in [KRaft mode](../../raft/index.md).

```shell
$ ./bin/kafka-storage.sh -h
usage: kafka-storage [-h] {info,format,random-uuid} ...

The Kafka storage tool.

positional arguments:
  {info,format,random-uuid}
    info                 Get information about the Kafka log directories on this node.
    format               Format the Kafka log directories on this node.
    random-uuid          Print a random UUID.

optional arguments:
  -h, --help             show this help message and exit
```

`kafka-storage` runs [kafka.tools.StorageTool](StorageTool.md).

## format

[format](StorageTool.md#format) command is used to format the Kafka log directories of a node.

```shell
$ ./bin/kafka-storage.sh format -h
usage: kafka-storage format [-h] --config CONFIG --cluster-id CLUSTER_ID [--add-scram ADD_SCRAM] [--ignore-formatted] [--release-version RELEASE_VERSION]

optional arguments:
  -h, --help             show this help message and exit
  --config CONFIG, -c CONFIG
                         The Kafka configuration file to use.
  --cluster-id CLUSTER_ID, -t CLUSTER_ID
                         The cluster ID to use.
  --add-scram ADD_SCRAM, -S ADD_SCRAM
                         A SCRAM_CREDENTIAL to add to the __cluster_metadata log e.g.
                         'SCRAM-SHA-256=[name=alice,password=alice-secret]'
                         'SCRAM-SHA-512=[name=alice,iterations=8192,salt="N3E=",saltedpassword="YCE="]'
  --ignore-formatted, -g
  --release-version RELEASE_VERSION, -r RELEASE_VERSION
                         A KRaft release version to use for the initial metadata version. The minimum is 3.0, the default is 3.6-IV2
```

`format` is the second command to be executed while setting up a Kafka cluster.

```shell
$ ./bin/kafka-storage.sh format -t $KAFKA_CLUSTER_ID -c config/kraft/server.properties
Formatting /tmp/kraft-combined-logs with metadata.version 3.6-IV2.
```

## info

```shell
$ ./bin/kafka-storage.sh info -h
usage: kafka-storage info [-h] --config CONFIG

optional arguments:
  -h, --help             show this help message and exit
  --config CONFIG, -c CONFIG
                         The Kafka configuration file to use.
```

```shell
$ ./bin/kafka-storage.sh info -c config/kraft/server.properties
Found log directory:
  /tmp/kraft-combined-logs

Found metadata: {cluster.id=F9_futKUQPKBwpQddvXsDQ, node.id=1, version=1}

$ tree /tmp/kraft-combined-logs
/tmp/kraft-combined-logs
├── bootstrap.checkpoint
└── meta.properties
```

## random-uuid

[random-uuid](StorageTool.md#random-uuid) command is used to generate a pseudo randomly generated UUID for a cluster UUID.

```shell
$ ./bin/kafka-storage.sh random-uuid
pnHyFfvWT6i2F2wZzSUx6A
```

`random-uuid` is the first command to be executed while setting up a Kafka cluster.

```shell
KAFKA_CLUSTER_ID="$(bin/kafka-storage.sh random-uuid)"
```
