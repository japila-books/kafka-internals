# ReplicaFetcher

`ReplicaFetcher` is a thread that [ReplicaVerificationTool](ReplicaVerificationTool.md) uses for [replica verification](#doWork-verification).

## Creating Instance

`ReplicaFetcher` takes the following to be created:

* <span id="name"> `ReplicaFetcher-[brokerId]`
* <span id="sourceBroker"> Source broker
* <span id="topicPartitions"> `TopicPartition`s
* <span id="topicIds"> Topics IDs (`Map[String, Uuid]`)
* <span id="replicaBuffer"> [ReplicaBuffer](ReplicaBuffer.md)
* <span id="socketTimeout"> Socket timeout (`30000`)
* <span id="socketBufferSize"> Socket buffer size (`256000`)
* <span id="fetchSize"> Fetch size
* <span id="maxWait"> Max wait
* <span id="minBytes"> Min bytes
* [doVerification](#doVerification) flag
* <span id="consumerConfig"> Consumer properties
* <span id="fetcherId"> Fetcher ID

`ReplicaFetcher` is created when:

* `ReplicaVerificationTool` command-line utility is [executed](ReplicaVerificationTool.md#main)

### <span id="doVerification"> doVerification

`ReplicaFetcher` is given `doVerification` flag when [created](#creating-instance).

`doVerification` flag is enabled for a single `ReplicaFetcher` among the replica fetcher threads.

The flag is used to determine which `ReplicaFetcher` should [perform verification](#doWork-verification).

## <span id="doWork"> doWork

```scala
doWork(): Unit
```

`doWork` is part of the `ShutdownableThread` abstraction.

---

`doWork` creates a `requestMap` with `TopicPartition`s and `PartitionData`s.

`doWork` prints out the following DEBUG message to the logs:

```text
Issuing fetch request
```

`doWork` sends a `Fetch` request (with the `requestMap`).

With a `FetchResponse`, `doWork` [addFetchedData](ReplicaBuffer.md#addFetchedData) (to the [ReplicaBuffer](#replicaBuffer) that all `ReplicaFetcher`s append fetched data to). Otherwise, `doWork`...FIXME

`doWork` decrements the [fetcherBarrier](ReplicaBuffer.md#getFetcherBarrier) latch. If it reaches 0, `doWork` prints out the following DEBUG message to the logs:

```text
Done fetching
```

`doWork` waits for the other fetchers to finish and prints out the following DEBUG message to the logs:

```text
Ready for verification
```

With the [doVerification](#doVerification) flag enabled, `doWork` [performs verification](#doWork-verification).

`doWork` waits for the verification to be finished and prints out the following DEBUG message to the logs:

```text
Done verification
```

### <span id="doWork-verification"> Verification

`doWork`...FIXME
