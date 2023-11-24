# Authorizer

`Authorizer` is an [abstraction](#contract) of [broker authorizers](#implementations) that Kafka brokers use to [authorize operations](#authorize) based on access-control list (ACL).

From Wikipedia's [Access-control list](https://en.wikipedia.org/wiki/Access-control_list):

> An **access-control list (ACL)** is a list of permissions attached to an object.
>
> An ACL specifies which users or system processes are granted access to objects,
> as well as what operations are allowed on given objects.
>
> Each entry in a typical ACL specifies a subject and an operation. For instance, if a file object has an ACL
> that contains (Alice: read,write; Bob: read), this would give Alice permission to read and write the file and
> Bob to only read it

`Authorizer` is configured by [authorizer.class.name](../KafkaConfig.md#authorizer.class.name) configuration property.

!!! note "KIP-504"
    `Authorizer` abstraction is part of [KIP-504 - Add new Java Authorizer Interface](https://cwiki.apache.org/confluence/display/KAFKA/KIP-504+-+Add+new+Java+Authorizer+Interface).

## Contract

### ACL Bindings { #acls }

```java
Iterable<AclBinding> acls(
  AclBindingFilter filter)
```

ACL bindings for the provided `filter` (synchronously)

Used when:

* `Authorizer` is requested to [authorizeByResourceType](Authorizer.md#authorizeByResourceType)
* `AuthorizerService` is requested to [getAcls](../tools/kafka-acls/AuthorizerService.md#getAcls)
* `AclApis` is requested to [handleDescribeAcls](AclApis.md#handleDescribeAcls)

### <span id="authorize"> Authorizing Request to Execute Actions

```java
List<AuthorizationResult> authorize(
  AuthorizableRequestContext requestContext,
  List<Action> actions)
```

Authorizes the actions performed by the request (synchronously)

Used when:

* `Authorizer` is requested to [authorizeByResourceType](Authorizer.md#authorizeByResourceType)
* `AuthHelper` is requested to [authorize](AuthHelper.md#authorize), [authorizedOperations](AuthHelper.md#authorizedOperations), [filterByAuthorized](AuthHelper.md#filterByAuthorized)

### <span id="createAcls"> createAcls

```java
List<? extends CompletionStage<AclCreateResult>> createAcls(
  AuthorizableRequestContext requestContext,
  List<AclBinding> aclBindings)
```

Creates new ACL bindings (asynchronously)

Used when:

* `AuthorizerService` is requested to [addAcls](../tools/kafka-acls/AuthorizerService.md#addAcls)
* `AclApis` is requested to [handleCreateAcls](AclApis.md#handleCreateAcls)

### <span id="deleteAcls"> deleteAcls

```java
List<? extends CompletionStage<AclDeleteResult>> deleteAcls(
  AuthorizableRequestContext requestContext,
  List<AclBindingFilter> aclBindingFilters)
```

Deletes all ACL bindings matching the `aclBindingFilters` filters (asynchronously)

Used when:

* `AuthorizerService` is requested to [removeAcls](../tools/kafka-acls/AuthorizerService.md#removeAcls)
* `AclApis` is requested to [handleDeleteAcls](AclApis.md#handleDeleteAcls)

### <span id="start"> start

```java
Map<Endpoint, ? extends CompletionStage<Void>> start(
  AuthorizerServerInfo serverInfo)
```

Starts loading authorization metadata (asynchronously)

Returns futures that can be used to wait until metadata for authorizing requests on each listener is available. The future returned for each listener must return only when authorizer is ready to authorize requests on the listener.

Used when:

* `BrokerServer` is requested to [start up](../kraft/BrokerServer.md#startup)
* `ControllerServer` is requested to [start up](../kraft/ControllerServer.md#startup)
* `KafkaServer` is requested to [start up](../broker/KafkaServer.md#startup)

## Implementations

* [AclAuthorizer](AclAuthorizer.md)
* [ClusterMetadataAuthorizer](ClusterMetadataAuthorizer.md)

## <span id="Configurable"> Configurable

`Authorizer` is a [Configurable](../Configurable.md).

## <span id="authorizeByResourceType"> authorizeByResourceType

```java
AuthorizationResult authorizeByResourceType(
  AuthorizableRequestContext requestContext,
  AclOperation op,
  ResourceType resourceType)
```

`authorizeByResourceType` [authorizes](#authorize) access to the `resourceType` by super users.

`authorizeByResourceType` creates a `KafkaPrincipal` (based on the `PrincipalType` and `Name` from the `requestContext`) and reads the request's host address. `authorizeByResourceType` tries to authorize the request based on the [ACL bindings](#acls) (with a `AclBindingFilter` for the `resourceType` and `ANY` pattern).

---

`authorizeByResourceType` is used when:

* `AuthHelper` is requested to [authorizeByResourceType](AuthHelper.md#authorizeByResourceType)
