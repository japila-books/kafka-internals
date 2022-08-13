# AclCommandService

`AclCommandService` is an [abstraction](#contract) of [AclCommand services](#implementations) for [AclCommand](AclCommand.md).

## Contract

### <span id="addAcls"> addAcls

```scala
addAcls(): Unit
```

Used when:

* [AclCommand](AclCommand.md) is executed (with `--add` option)

### <span id="listAcls"> listAcls

```scala
listAcls(): Unit
```

Used when:

* [AclCommand](AclCommand.md) is executed (with `--list` option)

### <span id="removeAcls"> removeAcls

```scala
removeAcls(): Unit
```

Used when:

* [AclCommand](AclCommand.md) is executed (with `--remove` option)

## Implementations

* [AdminClientService](AdminClientService.md)
* [AuthorizerService](AuthorizerService.md)
