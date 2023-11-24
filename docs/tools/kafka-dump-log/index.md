# kafka-dump-log Tool

`kafka-dump-log.sh` utility can dump log segments of a topic (incl. cluster metadata) to the console (e.g., for debugging a seemingly corrupt log segment).

`kafka-dump-log.sh` has got two new flags: [--cluster-metadata-decoder](#cluster-metadata-decoder), and [--skip-record-metadata](#skip-record-metadata).

```shell
./bin/kafka-dump-log.sh \
  --cluster-metadata-decoder \
  --skip-record-metadata \
  --files /tmp/kraft-controller-logs/__cluster_metadata-0/00000000000000000000.log,/tmp/kraft-controller-logs/__cluster_metadata-0/00000000000000000000.index
```

## cluster-metadata-decoder

`--cluster-metadata-decoder` flag tells the `DumpLogSegments` tool to decode the records as [cluster metadata](../../kraft/index.md) records.

## skip-record-metadata

`--skip-record-metadata` flag will skip printing metadata for each record.  However, metadata for each record batch will still be printed when this flag is present.
