# Demo: Controller Election

Use the following setup with one Zookeeper server and two Kafka brokers to observe the Kafka controller election.

Start Zookeeper server.

```console
$ ./bin/zookeeper-server-start.sh config/zookeeper.properties
...
INFO binding to port 0.0.0.0/0.0.0.0:2181 (org.apache.zookeeper.server.NIOServerCnxnFactory)
```

Add the following line to `config/log4j.properties` to enable `DEBUG` logging level for `kafka.controller.KafkaController` logger.

```text
log4j.logger.kafka.controller.KafkaController=DEBUG
```

Start a Kafka broker.

``` console
$ ./bin/kafka-server-start.sh config/server.properties \
    --override broker.id=100 \
    --override log.dirs=/tmp/kafka-logs-100 \
    --override port=9192
...
INFO Registered broker 100 at path /brokers/ids/100 with addresses: EndPoint(192.168.1.4,9192,ListenerName(PLAINTEXT),PLAINTEXT) (kafka.utils.ZkUtils)
INFO Kafka version : 1.0.0-SNAPSHOT (org.apache.kafka.common.utils.AppInfoParser)
INFO Kafka commitId : 852297efd99af04d (org.apache.kafka.common.utils.AppInfoParser)
INFO [KafkaServer id=100] started (kafka.server.KafkaServer)
```

Start another Kafka broker (with different properties)

``` console
$ ./bin/kafka-server-start.sh config/server.properties \
    --override broker.id=200 \
    --override log.dirs=/tmp/kafka-logs-200 \
    --override port=9292
...
INFO Registered broker 200 at path /brokers/ids/200 with addresses: EndPoint(192.168.1.4,9292,ListenerName(PLAINTEXT),PLAINTEXT) (kafka.utils.ZkUtils)
INFO Kafka version : 1.0.0-SNAPSHOT (org.apache.kafka.common.utils.AppInfoParser)
INFO Kafka commitId : 852297efd99af04d (org.apache.kafka.common.utils.AppInfoParser)
INFO [KafkaServer id=200] started (kafka.server.KafkaServer)
```

Connect to Zookeeper using Zookeeper CLI (command-line interface).

!!! tip
    Use the official distribution of [Apache Zookeeper](http://zookeeper.apache.org/releases.html).

    The zookeeper shell shipped with Kafka works with no support for command line history (because jline jar is missing, cf. [KAFKA-2385](https://issues.apache.org/jira/browse/KAFKA-2385)).

``` shell
$ ./bin/zkCli.sh -server localhost:2181
```

Once connected, execute `get /controller` to get the data associated with `/controller` znode where the active Kafka controller stores the controller ID.

```text
[zk: localhost:2181(CONNECTED) 0] get /controller
{"version":1,"brokerid":100,"timestamp":"1506423376977"}
cZxid = 0x191
ctime = Tue Sep 26 12:56:16 CEST 2017
mZxid = 0x191
mtime = Tue Sep 26 12:56:16 CEST 2017
pZxid = 0x191
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x15ebdd241840002
dataLength = 56
numChildren = 0
```

!!! tip
    Clear the consoles of the two Kafka brokers so you have the election logs only.

Delete `/controller` znode and observe the controller election.

``` text
[zk: localhost:2181(CONNECTED) 2] delete /controller
```

You should see the following in the logs in the consoles of the two Kafka brokers.

``` text
DEBUG [Controller id=100] Resigning (kafka.controller.KafkaController)
DEBUG [Controller id=100] De-registering IsrChangeNotificationListener (kafka.controller.KafkaController)
DEBUG [Controller id=100] De-registering logDirEventNotificationListener (kafka.controller.KafkaController)
INFO [Controller id=100] Resigned (kafka.controller.KafkaController)
DEBUG [Controller id=100] Broker 200 has been elected as the controller, so stopping the election process. (kafka.controller.KafkaController)
```

and

``` text
INFO Creating /controller (is it secure? false) (kafka.utils.ZKCheckedEphemeral)
INFO Result of znode creation is: OK (kafka.utils.ZKCheckedEphemeral)
INFO [Controller id=200] 200 successfully elected as the controller (kafka.controller.KafkaController)
INFO [Controller id=200] Starting become controller state transition (kafka.controller.KafkaController)
INFO [Controller id=200] Initialized controller epoch to 39 and zk version 38 (kafka.controller.KafkaController)
INFO [Controller id=200] Incremented epoch to 40 (kafka.controller.KafkaController)
DEBUG [Controller id=200] Registering IsrChangeNotificationListener (kafka.controller.KafkaController)
DEBUG [Controller id=200] Registering logDirEventNotificationListener (kafka.controller.KafkaController)
INFO [Controller id=200] Partitions being reassigned: Map() (kafka.controller.KafkaController)
INFO [Controller id=200] Partitions already reassigned: Set() (kafka.controller.KafkaController)
INFO [Controller id=200] Resuming reassignment of partitions: Map() (kafka.controller.KafkaController)
INFO [Controller id=200] Currently active brokers in the cluster: Set(100, 200) (kafka.controller.KafkaController)
INFO [Controller id=200] Currently shutting brokers in the cluster: Set() (kafka.controller.KafkaController)
INFO [Controller id=200] Current list of topics in the cluster: Set(my-topic2, NEW, my-topic, my-topic1) (kafka.controller.KafkaController)
INFO [Controller id=200] List of topics to be deleted:  (kafka.controller.KafkaController)
INFO [Controller id=200] List of topics ineligible for deletion:  (kafka.controller.KafkaController)
INFO [Controller id=200] Ready to serve as the new controller with epoch 40 (kafka.controller.KafkaController)
INFO [Controller id=200] Partitions undergoing preferred replica election:  (kafka.controller.KafkaController)
INFO [Controller id=200] Partitions that completed preferred replica election:  (kafka.controller.KafkaController)
INFO [Controller id=200] Skipping preferred replica election for partitions due to topic deletion:  (kafka.controller.KafkaController)
INFO [Controller id=200] Resuming preferred replica election for partitions:  (kafka.controller.KafkaController)
INFO [Controller id=200] Starting preferred replica leader election for partitions  (kafka.controller.KafkaController)
INFO [Controller id=200] Starting the controller scheduler (kafka.controller.KafkaController)
```
