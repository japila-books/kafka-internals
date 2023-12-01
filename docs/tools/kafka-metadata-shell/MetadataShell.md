# MetadataShell

`MetadataShell` is [launched](#main) as a command-line application using [kafka-metadata-shell.sh](index.md) shell script.

## Creating Instance

`MetadataShell` takes the following to be created:

* [KafkaRaftManager](#raftManager)
* <span id="snapshotPath"> Snapshot Path
* <span id="faultHandler"> `FaultHandler`

`MetadataShell` is created when:

* `MetadataShell.Builder` is requested to `build` (a `MetadataShell`)

### KafkaRaftManager { #raftManager }

`MetadataShell` is given a [KafkaRaftManager](../../kraft/KafkaRaftManager.md) when [created](#creating-instance).

!!! note
    A `KafkaRaftManager` does not seem to be defined ever (and seems always `null`).

## Entry Point { #main }

```scala
void main(
  String[] args)
```

`main`...FIXME

---

`main` is used when:

* FIXME
