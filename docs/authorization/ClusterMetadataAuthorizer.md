# ClusterMetadataAuthorizer

`ClusterMetadataAuthorizer` is an [extension](#contract) of the [Authorizer](Authorizer.md) abstraction for [authorizers](#implementations) that store state in the `__cluster_metadata` log.

## Contract (Subset)

### loadSnapshot { #loadSnapshot }

```java
void loadSnapshot(
  Map<Uuid, StandardAcl> acls)
```

See:

* [StandardAuthorizer](StandardAuthorizer.md#loadSnapshot)

Used when:

* `AclPublisher` is requested to [onMetadataUpdate](AclPublisher.md#onMetadataUpdate)

## Implementations

* [StandardAuthorizer](StandardAuthorizer.md)
