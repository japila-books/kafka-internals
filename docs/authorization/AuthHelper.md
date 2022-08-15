# AuthHelper

## Creating Instance

`AuthHelper` takes the following to be created:

* <span id="authorizer"> [Authorizer](Authorizer.md)

`AuthHelper` is created when:

* `ControllerApis` is created (`authHelper`)
* `KafkaApis` is [created](../KafkaApis.md#authHelper)

## <span id="authorizeClusterOperation"> authorizeClusterOperation

```scala
authorizeClusterOperation(
  request: RequestChannel.Request,
  operation: AclOperation): Unit
```

`authorizeClusterOperation`...FIXME

---

`authorizeClusterOperation` is used when:

* FIXME

## <span id="authorizeByResourceType"> authorizeByResourceType

```scala
authorizeByResourceType(
  requestContext: RequestContext,
  operation: AclOperation,
  resourceType: ResourceType): Boolean
```

`authorizeByResourceType`...FIXME

---

`authorizeByResourceType` is used when:

* FIXME
