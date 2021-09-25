# PartitionData

`PartitionData` is a `Message` of [FetchResponseData](FetchResponseData.md).

## <span id="abortedTransactions"> abortedTransactions

```java
List<AbortedTransaction> abortedTransactions
List<AbortedTransaction> abortedTransactions()
```

`abortedTransactions`...FIXME

`abortedTransactions` is used when:

* FIXME

## <span id="preferredReadReplica"> Preferred Read Replica

Default: `-1`

`preferredReadReplica` is used when:

* `Fetcher` is requested to [initializeCompletedFetch](clients/consumer/Fetcher.md#initializeCompletedFetch)
