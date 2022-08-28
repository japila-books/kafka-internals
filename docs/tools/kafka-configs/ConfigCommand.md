# ConfigCommand

`kafka.admin.ConfigCommand` is a [command-line utility](#main).

`ConfigCommand` can be executed using [kafka-configs](index.md) shell script.

## <span id="processCommandWithZk"> processCommandWithZk

```scala
processCommandWithZk(
  zkConnectString: String,
  opts: ConfigCommandOptions): Unit
```

`processCommandWithZk`...FIXME

---

`processCommandWithZk` is used when:

* `ConfigCommand` is [executed](#main) with `--zookeeper` option (deprecated)

## <span id="alterConfigWithZk"> alterConfigWithZk

```scala
alterConfigWithZk(
  zkClient: KafkaZkClient,
  opts: ConfigCommandOptions,
  adminZkClient: AdminZkClient): Unit
```

`alterConfigWithZk`...FIXME

---

`alterConfigWithZk` is used when:

* `ConfigCommand` is [executed](#processCommandWithZk) with `--alter` option (to alter the configuration for an entity)
