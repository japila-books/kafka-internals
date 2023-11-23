# QuorumStateStore

`QuorumStateStore` is an [abstraction](#contract) of [kraft quorum state stores](#implementations).

## Contract (Subset)

### readElectionState { #readElectionState }

```java
ElectionState readElectionState()
```

Reads (_loads_) the latest election state

See:

* [FileBasedStateStore](FileBasedStateStore.md#readElectionState)

Used when:

* `QuorumState` is requested to [initialize](QuorumState.md#initialize)

## Implementations

* [FileBasedStateStore](FileBasedStateStore.md)
