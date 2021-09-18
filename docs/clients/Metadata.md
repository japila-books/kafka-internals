# Metadata

## <span id="update"> update

```java
void update(
  int requestVersion,
  MetadataResponse response,
  boolean isPartialUpdate,
  long nowMs)
```

`update`...FIXME

`update` is used when:

* `ProducerMetadata` is requested to `update`
* `Metadata` is requested to [updateWithCurrentRequestVersion](#updateWithCurrentRequestVersion)
* `DefaultMetadataUpdater` is requested to `handleSuccessfulResponse`
