# AuthHelper

`AuthHelper` is used by `ControllerApis` and [KafkaApis](../KafkaApis.md#authHelper) to [authorize requests](#authorize) to execute an operation on a resource (by type and name).

## Creating Instance

`AuthHelper` takes the following to be created:

* <span id="authorizer"> [Authorizer](Authorizer.md)

`AuthHelper` is created when:

* `ControllerApis` is created (`authHelper`)
* `KafkaApis` is [created](../KafkaApis.md#authHelper)

## <span id="authorize"> Authorizing Request

```scala
authorize(
  requestContext: RequestContext,
  operation: AclOperation,
  resourceType: ResourceType,
  resourceName: String,
  logIfAllowed: Boolean = true,
  logIfDenied: Boolean = true,
  refCount: Int = 1): Boolean
```

`authorize` requests the [Authorizer](#authorizer) (if defined) to [authorize](Authorizer.md#authorize) the request (to execute the `AclOperation` on a resource by [ResourceType](index.md#ResourceType) and `resourceName`).

---

`authorize` is used when:

Kafka Service | Request | AclOperation | ResourceType | Resource Name
--------------|---------|--------------|--------------|---------------
 `AuthHelper` | [authorizeClusterOperation](#authorizeClusterOperation) | | `CLUSTER` | kafka-cluster
 `ControllerApis` | _FIXME_ | &nbsp; | &nbsp; | &nbsp;
 `KafkaApis` | [handleOffsetCommitRequest](../KafkaApis.md#handleOffsetCommitRequest) | READ | GROUP | groupId
 &nbsp; | _FIXME_ | &nbsp; | &nbsp; | &nbsp;

## <span id="authorizeByResourceType"> authorizeByResourceType

```scala
authorizeByResourceType(
  requestContext: RequestContext,
  operation: AclOperation,
  resourceType: ResourceType): Boolean
```

`authorizeByResourceType` requests the [Authorizer](#authorizer) (if defined) to [authorizeByResourceType](Authorizer.md#authorizeByResourceType).

---

`authorizeByResourceType` is used when:

* `KafkaApis` is requested to [handleInitProducerIdRequest](../KafkaApis.md#handleInitProducerIdRequest) (to authorize `AclOperation.WRITE` action on `ResourceType.TOPIC`)

## <span id="authorizeClusterOperation"> authorizeClusterOperation

```scala
authorizeClusterOperation(
  request: RequestChannel.Request,
  operation: AclOperation): Unit
```

`authorizeClusterOperation` [authorizes](#authorize) the given `AclOperation` with `CLUSTER` resource type and (hardcoded) `kafka-cluster` name.

If access is denied, `authorizeClusterOperation` throws a `ClusterAuthorizationException`:

```text
Request [request] is not authorized.
```

---

`authorizeClusterOperation` is used when:

Kafka Service | Request | AclOperation
--------------|---------|-------------
 `AclApis` | [handleCreateAcls](AclApis.md#handleCreateAcls)     | `ALTER`
 &nbsp;    | [handleDeleteAcls](AclApis.md#handleDeleteAcls)     | `ALTER`
 &nbsp;    | [handleDescribeAcls](AclApis.md#handleDescribeAcls) | `DESCRIBE`
 `ControllerApis` | _FIXME_ |
 `KafkaApis` | [handleLeaderAndIsrRequest](../KafkaApis.md#handleLeaderAndIsrRequest) | `CLUSTER_ACTION`
 &nbsp; | [handleStopReplicaRequest](../KafkaApis.md#handleStopReplicaRequest) | `CLUSTER_ACTION`
 &nbsp; | [handleUpdateMetadataRequest](../KafkaApis.md#handleUpdateMetadataRequest) | `CLUSTER_ACTION`
 &nbsp; | [handleControlledShutdownRequest](../KafkaApis.md#handleControlledShutdownRequest) | `CLUSTER_ACTION`
 &nbsp; | [handleWriteTxnMarkersRequest](../KafkaApis.md#handleWriteTxnMarkersRequest) | `CLUSTER_ACTION`
 &nbsp; | [handleAlterPartitionRequest](../KafkaApis.md#handleAlterPartitionRequest) | `CLUSTER_ACTION`
 &nbsp; | [handleAllocateProducerIdsRequest](../KafkaApis.md#handleAllocateProducerIdsRequest) | `CLUSTER_ACTION`
 &nbsp; | [handleAlterPartitionReassignmentsRequest](../KafkaApis.md#handleAlterPartitionReassignmentsRequest) | `ALTER`
 &nbsp; | [handleListPartitionReassignmentsRequest](../KafkaApis.md#handleListPartitionReassignmentsRequest) | `DESCRIBE`
