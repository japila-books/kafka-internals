# KafkaZkClient

## <span id="getEntityConfigs"> getEntityConfigs

```scala
getEntityConfigs(
  rootEntityType: String,
  sanitizedEntityName: String): Properties
```

`getEntityConfigs` talks to Zookeeper for `config` data in the `/config/[entityType]/[entityName]` znode.

``` console
$ ./bin/zkCli.sh -server localhost:2181 ls /config
[brokers, changes, clients, ips, topics, users]
```

---

`getEntityConfigs` is used when:

* `AdminZkClient` is requested to [fetchEntityConfig](AdminZkClient.md#fetchEntityConfig)
