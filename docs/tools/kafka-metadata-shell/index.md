# kafka-metadata-shell Tool

`kafka-metadata-shell.sh` utility is a Kafka metadata tool to interactively examine the metadata stored in a [KRaft cluster](../../kraft/index.md).

`kafka-metadata-shell.sh` can read the metadata from a metadata snapshot on disk.

```shell
./bin/kafka-metadata-shell.sh \
  --snapshot /tmp/kraft-controller-logs/__cluster_metadata-0/*.snapshot
```

The metadata tool works by replaying the log and storing the state into in-memory nodes.
These nodes are presented in a fashion similar to filesystem directories.

`kafka-metadata-shell.sh` uses [MetadataShell](MetadataShell.md) as an entry point.

## Arguments

### snapshot

`--snapshot` (`-s`) is a metadata snapshot file to read.
