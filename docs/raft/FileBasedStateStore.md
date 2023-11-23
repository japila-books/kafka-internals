# FileBasedStateStore

`FileBasedStateStore` is a [QuorumStateStore](QuorumStateStore.md) that [writes election state](#writeElectionState) to a [file](#stateFile).

## Creating Instance

`FileBasedStateStore` takes the following to be created:

* [State File](#stateFile)

`FileBasedStateStore` is created when:

* `KafkaRaftManager` is requested to [build a RaftClient](KafkaRaftManager.md#buildRaftClient)

## State File { #stateFile }

`FileBasedStateStore` is given a **state file** when [created](#creating-instance).

The state file is `quorum-state` file under the [data directory](KafkaRaftManager.md#dataDir) (of [KafkaRaftManager](KafkaRaftManager.md)).

The state file is loaded in [readElectionState](#readElectionState) and updated in [writeElectionState](#writeElectionState).

`stateFile` is deleted in [clear](#clear).

## readElectionState { #readElectionState }

??? note "QuorumStateStore"

    ```java
    ElectionState readElectionState()
    ```

    `readElectionState` is part of the [QuorumStateStore](QuorumStateStore.md#readElectionState) abstraction.

`readElectionState` is `null` (undefined) when [stateFile](#stateFile) does not exist.
