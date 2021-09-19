# NetworkClient

`NetworkClient` is a [KafkaClient](KafkaClient.md).

## <span id="leastLoadedNode"> leastLoadedNode

```scala
Node leastLoadedNode(
  long now)
```

`leastLoadedNode` requests the [MetadataUpdater](#metadataUpdater) for the [nodes](MetadataUpdater.md#fetchNodes) (in a non-blocking fashion).

`leastLoadedNode` generates a random number to offset the first node to start checking node candidates from.

`leastLoadedNode` finds three nodes:

1. `foundReady` that is ready (perhaps with some [in-flight requests](#inFlightRequests))
1. `foundConnecting` with a connection already being established (using the `ClusterConnectionStates` registry)
1. `foundCanConnect` that [can be connected](#canConnect)

When a node is found that is ready and has no [in-flight requests](#inFlightRequests), `leastLoadedNode` prints out the following TRACE message to the logs and returns the node immediately:

```text
Found least loaded node [node] connected with no in-flight requests
```

When a node candidate does not meet any of the above requirements, `leastLoadedNode` prints out the following TRACE message to the logs:

```text
Removing node [node] from least loaded node selection since it is neither ready for sending nor connecting
```

`leastLoadedNode` prefers the `foundReady` node over the `foundConnecting` with the `foundCanConnect` as the last resort.

When no node could be found, `leastLoadedNode` prints out the following TRACE message to the logs (and returns `null`):

```text
Least loaded node selection failed to find an available node
```

`leastLoadedNode` is part of the [KafkaClient](KafkaClient.md#leastLoadedNode) abstraction.
