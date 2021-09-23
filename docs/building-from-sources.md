# Building from Sources

Based on [README.md](https://github.com/apache/kafka/blob/2.8.0/README.md#apache-kafka):

```text
KAFKA_VERSION={{ kafka.version }}
SCALA_VERSION={{ scala.short_version }}
```

```text
$ java -version
openjdk version "11.0.12" 2021-07-20
OpenJDK Runtime Environment Temurin-11.0.12+7 (build 11.0.12+7)
OpenJDK 64-Bit Server VM Temurin-11.0.12+7 (build 11.0.12+7, mixed mode)
```

```text
./gradlew clean releaseTarGz install -PskipSigning=true && \
tar -zxvf core/build/distributions/kafka_$SCALA_VERSION-$KAFKA_VERSION.tgz
```

```text
cd kafka_$SCALA_VERSION-$KAFKA_VERSION
```

```text
$ ./bin/kafka-server-start.sh --version | tail -1
3.0.0 (Commit:8cb0a5e9d3441962)
```
