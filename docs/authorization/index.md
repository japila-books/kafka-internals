# Authorization

**Kafka Authorization** is based on the following:

* [kafka-acls.sh](AclCommand.md) utility for ACL management
* [authorizer.class.name](../KafkaConfig.md#authorizer.class.name) configuration property
* [Authorizer](Authorizer.md) (and [AclAuthorizer](AclAuthorizer.md))

## Resource Types

 Type Name | Resource
-----------|---------
 ANY | Any resource
 TOPIC | A topic
 GROUP | [Consumer group](../consumer-groups/index.md)
 CLUSTER | Kafka cluster as a whole
 TRANSACTIONAL_ID | [Transactional ID](../transactions/index.md)
 DELEGATION_TOKEN | A delegation token
