# MetadataPublisher

`MetadataPublisher` is an [abstraction](#contract) of [metadata publishers](#implementations).

## Contract (Subset)

### onMetadataUpdate { #onMetadataUpdate }

```java
void onMetadataUpdate(
  MetadataDelta delta,
  MetadataImage newImage,
  LoaderManifest manifest)
```

See:

* [AclPublisher](../authorization/AclPublisher.md#onMetadataUpdate)
* [BrokerMetadataPublisher](BrokerMetadataPublisher.md#onMetadataUpdate)

Used when:

* `MetadataLoader` is requested to [initializeNewPublishers](MetadataLoader.md#initializeNewPublishers) and [maybePublishMetadata](MetadataLoader.md#maybePublishMetadata)
* `BrokerMetadataPublisher` is requested to [onMetadataUpdate](BrokerMetadataPublisher.md#onMetadataUpdate)

## Implementations

* [AclPublisher](../authorization/AclPublisher.md)
* [BrokerMetadataPublisher](BrokerMetadataPublisher.md)
* `ControllerMetadataMetricsPublisher`
* `DelegationTokenPublisher`
* `DynamicClientQuotaPublisher`
* `DynamicConfigPublisher`
* `FeaturesPublisher`
* `KRaftMigrationDriver`
* [MetadataShellPublisher](../tools/kafka-metadata-shell/MetadataShellPublisher.md)
* `ScramPublisher`
* `SnapshotGenerator`
