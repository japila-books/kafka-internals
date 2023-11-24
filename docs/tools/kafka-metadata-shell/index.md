# kafka-metadata-shell Tool

`kafka-metadata-shell.sh` utility is a Kafka metadata tool to interactively examine the metadata stored in a [KRaft cluster](../../raft/index.md).

```shell
./bin/kafka-metadata-shell.sh \
  --snapshot /tmp/kraft-controller-logs/__cluster_metadata-0/*.snapshot
```

`kafka-metadata-shell.sh` uses [MetadataShell](MetadataShell.md) as an entry point.

## Arguments

### snapshot

`--snapshot` (`-s`) is a metadata snapshot file to read.
