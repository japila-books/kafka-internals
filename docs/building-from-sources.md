# Building from Sources

Based on [README.md](https://github.com/apache/kafka/blob/2.8.0/README.md#apache-kafka):

```text
KAFKA_VERSION=2.8.0
```

```text
./gradlew clean releaseTarGz install && \
tar -zxvf core/build/distributions/kafka_2.13-$KAFKA_VERSION.tgz)
```

```text
cd kafka_2.13-$KAFKA_VERSION
```

```text
$ ./bin/kafka-server-start.sh --version
[2021-09-12 19:00:12,467] INFO Registered kafka:type=kafka.Log4jController MBean (kafka.utils.Log4jControllerRegistration$)
2.8.0 (Commit:ebb1d6e21cc92130)
```
