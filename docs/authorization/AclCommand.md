# AclCommand

`AclCommand` is an [administration command-line utility](#main) to manage ACLs in a Kafka cluster.

`AclCommand` can be executed as `kafka-acls` shell script.

## <span id="SecurityDisabledException"> SecurityDisabledException

`kafka-acls.sh` requires [Authorizer](Authorizer.md) to be configured on a broker (when executed with `--bootstrap-server` option) or throws a `SecurityDisabledException`.

``` console
$ ./bin/kafka-acls.sh --list --bootstrap-server :9092
SecurityDisabledException: No Authorizer is configured on the broker
```

## <span id="main"> Executing Command

`main` selects the [AclCommandService](AclCommandService.md):

* `AdminClientService` when `--bootstrap-server` option is used
* [AuthorizerService](AuthorizerService.md) with [AclAuthorizer](AclAuthorizer.md) otherwise

In the end, `main` executes the operation:

* `add` to [add ACLs](AclCommandService.md#addAcls)
* `remove` to [remove ACLs](AclCommandService.md#removeAcls)
* `list` to [list ACLs](AclCommandService.md#listAcls) for `--topic`, `--group` or `--cluster` resource types
