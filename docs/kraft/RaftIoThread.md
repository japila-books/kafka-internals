# RaftIoThread

`RaftIoThread` is an uninterruptible `ShutdownableThread` with the following name (based on the [threadNamePrefix](#threadNamePrefix)):

```text
[threadNamePrefix]-io-thread
```

## Creating Instance

`RaftIoThread` takes the following to be created:

* <span id="client"> [KafkaRaftClient](KafkaRaftClient.md)
* <span id="threadNamePrefix"> Thread Name Prefix
* <span id="fatalFaultHandler"> `FaultHandler`

`RaftIoThread` is created alongside [KafkaRaftManager](KafkaRaftManager.md#raftIoThread).

## doWork { #doWork }

??? note "ShutdownableThread"

    ```scala
    doWork(): Unit
    ```

    `doWork` is part of the `ShutdownableThread` abstraction.

`doWork` requests the [KafkaRaftClient](#client) to [poll](KafkaRaftClient.md#poll).
