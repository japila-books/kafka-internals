---
hide:
  - navigation
---

# Demo: Kafka and kcat in Docker

This demo uses Docker to run Apache Kafka and [kcat](https://github.com/edenhill/kcat) utility.

## kafka-docker

Pull [kafka-docker](https://github.com/wurstmeister/kafka-docker) project (or create a `docker-compose.yml` file yourself).

### Running Kafka Cluster

Start Zookeeper and Kafka containers.

```text
docker-compose up
```

```text
$ docker-compose ps
          Name                        Command               State                                  Ports
----------------------------------------------------------------------------------------------------------------------------------------
kafka-docker_kafka_1       start-kafka.sh                   Up      0.0.0.0:62687->9092/tcp
kafka-docker_zookeeper_1   /bin/sh -c /usr/sbin/sshd  ...   Up      0.0.0.0:2181->2181/tcp,:::2181->2181/tcp, 22/tcp, 2888/tcp, 3888/tcp
```

### Docker Network

The above creates a Docker network `kafka-docker_default` (if ran from `kafka-docker` directory as described in the [official documentation of docker-compose](https://docs.docker.com/compose/networking/)).

```text
$ docker network ls
NETWORK ID     NAME                   DRIVER    SCOPE
b8b255710858   bridge                 bridge    local
3c9c3a969ef2   cda                    bridge    local
398f9f3196aa   host                   host      local
68611503fde8   kafka-docker_default   bridge    local
db43a5e50281   none                   null      local
```

## kcat

Connect `kcat` container to the network (using `--network` option as described in the [official documentation of docker-compose](https://docs.docker.com/engine/reference/commandline/network_connect/#connect-a-container-to-a-network-when-it-starts)).

### Metadata Listing

```text
docker run -it --rm \
  --network kafka-docker_default \
  edenhill/kcat:1.7.0 \
  -b kafka-docker_kafka_1:9092 -L
```

```text
Metadata for all topics (from broker -1: kafka-docker_kafka_1:9092/bootstrap):
 1 brokers:
  broker 1001 at 09cc8de4d067:9092 (controller)
 0 topics:
```

### Producer

```text
docker run -it --rm \
  --network kafka-docker_default \
  --name producer \
  edenhill/kcat:1.7.0 \
  -b kafka-docker_kafka_1:9092 -P -t t1
```

!!! caution
    For some reason the above command couldn't send messages whenever I pressed ENTER but expected `Ctrl+D` instead (that terminates the shell and the container). Switching to [confluentinc/cp-kafkacat](https://hub.docker.com/r/confluentinc/cp-kafkacat/) made things working fine.

```text
docker run -it --rm \
  --network kafka-docker_default \
  --name producer \
  confluentinc/cp-kafkacat \
  kafkacat \
  -b kafka-docker_kafka_1:9092 -P -t t1
```

### Consumer

```text
docker run -it --rm \
  --network kafka-docker_default \
  --name consumer \
  edenhill/kcat:1.7.0 \
  -b kafka-docker_kafka_1:9092 -C -t t1
```

## Clean Up

```text
docker-compose down
```
