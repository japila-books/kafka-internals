# StorageTool

`StorageTool` (`kafka.tools.StorageTool`) is an utility to...FIXME

`StorageTool` can be executed using [bin/kafka-storage.sh](index.md) script.

## random-uuid

## format

`format` command [configToLogDirectories](#configToLogDirectories).

## configToLogDirectories { #configToLogDirectories }

```scala
configToLogDirectories(
  config: KafkaConfig): Seq[String]
```

`configToLogDirectories` takes the value of the following configuration properties:

* [log.dirs or log.dir](../../KafkaConfig.md#logDirs) (depending on availability)
* [metadataLogDir](../../KafkaConfig.md#metadataLogDir)
