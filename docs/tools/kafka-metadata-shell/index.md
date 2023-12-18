# kafka-metadata-shell Tool

`kafka-metadata-shell.sh` utility is a Kafka metadata tool to interactively examine the metadata stored in a [KRaft cluster](../../kraft/index.md).

`kafka-metadata-shell.sh` can read the metadata from a metadata snapshot on disk.

!!! note "KafkaRaftManager Not Supported Yet"
    There is no support to specify a [KafkaRaftManager](../../kraft/KafkaRaftManager.md) on command line yet.

Based on [metadata.log.max.snapshot.interval.ms](../../KafkaConfig.md#metadata.log.max.snapshot.interval.ms) and [metadata.log.max.record.bytes.between.snapshots](../../KafkaConfig.md#metadata.log.max.record.bytes.between.snapshots), KRaft controllers write metadata snapshots to cluster [metadata topic](../../kraft/index.md)'s log directory (using [SnapshotGenerator](../../metadata/SnapshotGenerator.md) and `SnapshotEmitter`).

```text
INFO [SnapshotGenerator id=1] Creating new KRaft snapshot file snapshot 00000000000000061020-0000000002 because we have waited at least 60 minute(s).
INFO [SnapshotEmitter id=1] Successfully wrote snapshot 00000000000000061020-0000000002
```

Use [--snapshot](#snapshot) option to load the snapshot file.

```console
$ ./bin/kafka-metadata-shell.sh \
  --snapshot /tmp/kraft-controller-logs/__cluster_metadata-0/00000000000000061020-0000000002.checkpoint
Loading...
Starting...
[ Kafka Metadata Shell ]
>>
```

```text
>> help
Welcome to the Apache Kafka metadata shell.

usage:  {cat,cd,exit,find,help,history,ls,man,pwd,tree} ...

positional arguments:
  {cat,cd,exit,find,help,history,ls,man,pwd,tree}
    cat                  Show the contents of metadata files.
    cd                   Set the current working directory.
    exit                 Exit the metadata shell.
    find                 Search for nodes in the directory hierarchy.
    help                 Display this help message.
    history              Print command history.
    ls                   List metadata nodes.
    man                  Show the help text for a specific command.
    pwd                  Print the current working directory.
    tree                 Show the contents of metadata nodes in a tree structure.
```

```text
>> ls
image  local
```

The metadata tool works by replaying the log and storing the state into in-memory nodes.
These nodes are presented in a fashion similar to filesystem directories.

```text
>> man tree
tree: Show the contents of metadata nodes in a tree structure.

usage: tree targets [targets ...]

positional arguments:
  targets                The metadata nodes to display.
```

```text
>> tree local
commitId:
  60e845626d8a465a
version:
  3.6.0
```

`kafka-metadata-shell.sh` uses [MetadataShell](MetadataShell.md) as an entry point.

## Arguments

### snapshot

`--snapshot` (`-s`) is a metadata snapshot file to read.
