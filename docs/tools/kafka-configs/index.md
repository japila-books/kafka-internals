# kafka-configs

`kafka-configs` utility uses [ConfigCommand](ConfigCommand.md) to...FIXME

## alterBrokerConfig

```console
./bin/kafka-configs.sh \
  --bootstrap-server :9092 \
  --alter \
  --entity-type brokers \
  --entity-name 0 \
  --add-config advertised.listeners=plaintext://:9092
```

## processBrokerConfig

```console
./bin/kafka-configs.sh \
  --bootstrap-server :9092 \
  --describe \
  --entity-type brokers \
  --entity-name 0
```
