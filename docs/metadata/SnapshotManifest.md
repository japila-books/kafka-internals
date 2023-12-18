# SnapshotManifest

`SnapshotManifest` is a [LoaderManifest](LoaderManifest.md).

## Creating Instance

`SnapshotManifest` takes the following to be created:

* <span id="provenance"> `MetadataProvenance`
* <span id="elapsedNs"> `elapsedNs`

`SnapshotManifest` is created when:

* `MetadataLoader` is requested to [initializeNewPublishers](MetadataLoader.md#initializeNewPublishers) and [loadSnapshot](MetadataLoader.md#loadSnapshot)

## LoaderManifestType { #type }

??? note "PARENT"

    ```java
    LoaderManifestType type()
    ```

    `type` is part of the [LoaderManifest](LoaderManifest.md#type) abstraction.

`type` is `SNAPSHOT`.
