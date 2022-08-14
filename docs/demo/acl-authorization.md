# Demo: ACL Authorization

This demo shows how to use [ACL authorization](../authorization/index.md) in Apache Kafka (using [AclAuthorizer](../authorization/AclAuthorizer.md)) to restrict topic operations.

In ACL words, the demo shows how to allow `Write` and `Read` operations to `ANY` topic to certain users.

You'll be using there users for different ACLs:

* `CN=root` - a user with all topic operations allowed

* `CN=producer` - a user with Write operation allowed

* `CN=consumer` - a user with Read operation allowed

## Before You Begin

The demo is a follow-up to [Demo: SSL Authentication](ssl-authentication.md). Please finish it first before this demo.

## Enable ACL Authorization

Enable ACL authorization in a Kafka cluster.

Add the following configuration property to `config/server-ssl.properties`:

```text
authorizer.class.name=kafka.security.authorizer.AclAuthorizer
```

Restart the Kafka broker and observe the logs. You should find the following `ClusterAuthorizationException` at the very end of the logs:

```text
ClusterAuthorizationException: Request Request(processor=0, connectionId=127.0.0.1:9093-127.0.0.1:62402-0, session=Session(User:CN=localhost,/127.0.0.1), listenerName=ListenerName(SSL), securityProtocol=SSL, buffer=null) is not authorized.
```

That's because `User:CN=localhost` user is not authorized to execute an action (since by default no one is allowed to execute any action).

## Review Authorization Logs

Access denials are logged at INFO level to `logs/kafka-authorizer.log` by default.

You should find the following INFO message (which corresponds to the `ClusterAuthorizationException` earlier):

```text
Principal = User:CN=localhost is Denied Operation = ClusterAction from host = 127.0.0.1 on resource = Cluster:LITERAL:kafka-cluster for request = UpdateMetadata with resourceRefCount = 1
```

## Enable DEBUG Logging Level for kafka.authorizer.logger

Enable allowed accesses that are logged at DEBUG level. Edit `config/log4j.properties` and change the logging level of `kafka.authorizer.logger` to DEBUG:

```text
log4j.logger.kafka.authorizer.logger=DEBUG, authorizerAppender
log4j.additivity.kafka.authorizer.logger=false
```

Restart the broker.

## Create User CN=root

You will now "create" a user identified as `CN=root`.

Generate the keys and certificate of the user.

```shell
keytool \
  -genkey \
  -keystore root.keystore \
  -alias root \
  -dname CN=root \
  -keyalg RSA \
  -validity 365 \
  -storepass 123456
```

Export the certificate of the user from the keystore.

```shell
keytool \
  -certreq \
  -keystore root.keystore \
  -alias root \
  -file root.unsigned.crt \
  -storepass 123456
```

Sign the certificate signing request with the root CA.

``` console
$ openssl x509 \
  -req \
  -CA ca.crt \
  -CAkey ca.key \
  -in root.unsigned.crt \
  -out root.crt \
  -days 365 \
  -CAcreateserial \
  -passin pass:1234
Signature ok
subject=CN = root
Getting CA Private Key
```

Import the certificate of the CA into the user keystore.

``` console
$ keytool \
  -importcert \
  -file ca.crt \
  -alias ca \
  -keystore root.keystore \
  -storepass 123456 \
  -noprompt
Certificate was added to keystore
```

Import the signed certificate into the user keystore. Make sure to use the same `-alias` as you used ealier.

``` console
$ keytool \
  -importcert \
  -file root.crt \
  -alias root \
  -keystore root.keystore \
  -storepass 123456
Certificate reply was installed in keystore
```

## Define Super Users

Super users are allowed to perform any operation on any resource in a Kafka cluster.

Define the broker (as `User:CN=localhost`) and `CN=root` as super users.

Add the following configuration property to `config/server-ssl.properties`. Note that the delimiter is semicolon (`;`) since user names may contain comma.

```text
super.users=User:CN=localhost;User:CN=root
```

Restart the broker.

There should be no exceptions in the logs.

Moreover, `logs/kafka-authorizer.log` should have the following DEBUG messages:

```text
principal = User:CN=localhost is a super user, allowing operation without checking acls.
Principal = User:CN=localhost is Allowed Operation = ClusterAction from host = 127.0.0.1 on resource = Cluster:LITERAL:kafka-cluster for request = UpdateMetadata with resourceRefCount = 1
```

## (Optional) Allow Everyone If No ACL Found

This step is optional.

For a less-secure broker configuration, you could add the following configuration property to `config/server-ssl.properties`:

```text
allow.everyone.if.no.acl.found=true
```

That would make access more open to any client (with a valid and trusted certificate).

For the demo, enable it so you won't run into the following `GroupAuthorizationException` later:

```text
GroupAuthorizationException: Not authorized to access group: console-consumer-...
```

Edit `config/server-ssl.properties` and restart the broker.

## List ACLs

Use [kafka-acls](../authorization/AclCommand.md) utility to list the access control list (ACL). There should be none.

Create `root.properties` as a minimal configuration of a Kafka client to identify itself as `CN=root`.

```text
security.protocol=SSL
ssl.truststore.location=/tmp/kafka-ssl-demo/client.truststore
ssl.truststore.password=123456
ssl.keystore.location=/tmp/kafka-ssl-demo/root.keystore
ssl.keystore.password=123456
ssl.key.password=123456
```

Use `--command-config` option to specify the SSL configuration.

```shell
kafka-acls.sh \
  --bootstrap-server :9093 \
  --list \
  --command-config /tmp/kafka-ssl-demo/root.properties
```

## Create User CN=producer

You will now "create" a `CN=producer` user (that will have Write operation allowed).

Generate the keys and certificate of a Kafka client to be authenticated as *CN=producer*.

``` shell
keytool \
  -genkey \
  -keystore producer.keystore \
  -alias producer \
  -dname CN=producer \
  -keyalg RSA \
  -validity 365 \
  -storepass 123456
```

Export the user certificate from the keystore.

``` shell
keytool \
  -certreq \
  -keystore producer.keystore \
  -alias producer \
  -file producer.unsigned.crt \
  -storepass 123456
```

Sign the certificate signing request with the root CA.

``` console
$ openssl x509 \
  -req \
  -CA ca.crt \
  -CAkey ca.key \
  -in producer.unsigned.crt \
  -out producer.crt \
  -days 365 \
  -CAcreateserial \
  -passin pass:1234
Signature ok
subject=CN = producer
Getting CA Private Key
```

Import the certificate of the CA into the user keystore.

``` console
$ keytool \
  -import \
  -file ca.crt \
  -keystore producer.keystore \
  -alias ca \
  -storepass 123456 \
  -noprompt
Certificate was added to keystore
```

Import the signed certificate into the user keystore. Make sure to use the same `-alias` as you used ealier.

``` console
$ keytool \
  -import \
  -file producer.crt \
  -keystore producer.keystore \
  -alias producer \
  -storepass 123456 \
  -noprompt
Certificate reply was installed in keystore
```

## Create User CN=consumer

You will now "create" a `CN=consumer` user (that will have Read operation allowed only).

Generate the keys and certificate of a Kafka client to be authenticated as *CN=consumer*.

```shell
keytool \
  -genkey \
  -keystore consumer.keystore \
  -alias consumer \
  -dname CN=consumer \
  -keyalg RSA \
  -validity 365 \
  -storepass 123456
```

Export the user certificate from the keystore.

```shell
keytool \
  -certreq \
  -keystore consumer.keystore \
  -alias consumer \
  -file consumer.unsigned.crt \
  -storepass 123456
```

Sign the certificate signing request with the root CA.

```console
$ openssl x509 \
  -req \
  -CA ca.crt \
  -CAkey ca.key \
  -in consumer.unsigned.crt \
  -out consumer.crt \
  -days 365 \
  -CAcreateserial \
  -passin pass:1234
Signature ok
subject=CN = consumer
Getting CA Private Key
```

Import the certificate of the CA into the user keystore.

```console
$ keytool \
  -import \
  -file ca.crt \
  -alias ca \
  -keystore consumer.keystore \
  -storepass 123456 \
  -noprompt
Certificate was added to keystore
```

Import the signed certificate into the user keystore. Make sure to use the same `-alias` as you used ealier.

```console
$ keytool \
  -import \
  -file consumer.crt \
  -alias consumer \
  -keystore consumer.keystore \
  -storepass 123456
Certificate reply was installed in keystore
```

## Restrict Topic Operations -- Write for CN=produce

Use [kafka-acls](../authorization/AclCommand.md) utility to restrict `Write` operation on any topic to `CN=produce` user (and super users).

```shell
kafka-acls.sh \
  --bootstrap-server :9093 \
  --add \
  --allow-principal User:CN=producer \
  --operation Write \
  --topic '*' \
  --command-config /tmp/kafka-ssl-demo/root.properties
```

List the ACLs using `kafka-acls` utility.

```console
$ kafka-acls.sh \
  --bootstrap-server :9093 \
  --list \
  --command-config /tmp/kafka-ssl-demo/root.properties
Current ACLs for resource `ResourcePattern(resourceType=TOPIC, name=*, patternType=LITERAL)`:
  (principal=User:CN=producer, host=*, operation=WRITE, permissionType=ALLOW)
```

## Restrict Topic Operations -- Read for CN=consumer

Use [kafka-acls](../authorization/AclCommand.md) utility to restrict `Write` operation on any topic to `CN=consumer` user (and super users).

```shell
kafka-acls.sh \
  --bootstrap-server :9093 \
  --add \
  --allow-principal User:CN=consumer \
  --operation Read \
  --topic '*' \
  --command-config /tmp/kafka-ssl-demo/root.properties
```

List the ACLs using `kafka-acls` utility.

```console
$ kafka-acls.sh \
  --bootstrap-server :9093 \
  --list \
  --command-config /tmp/kafka-ssl-demo/root.properties
Current ACLs for resource `ResourcePattern(resourceType=TOPIC, name=*, patternType=LITERAL)`:
  (principal=User:CN=producer, host=*, operation=WRITE, permissionType=ALLOW)
  (principal=User:CN=consumer, host=*, operation=READ, permissionType=ALLOW)
```

## Send Messages

Create `producer.properties` file as a minimal configuration of a Kafka client to use SSL authentication and identify itself as `CN=producer`:

```text
security.protocol=SSL
ssl.truststore.location=/tmp/kafka-ssl-demo/client.truststore
ssl.truststore.password=123456
ssl.keystore.location=/tmp/kafka-ssl-demo/producer.keystore
ssl.keystore.password=123456
ssl.key.password=123456
```

Use `kafka-console-producer.sh` utility to send a message to the Kafka broker as `CN=producer`:

```shell
kafka-console-producer.sh \
  --broker-list :9093 \
  --topic ssl \
  --producer.config /tmp/kafka-ssl-demo/producer.properties
```

In `logs/kafka-authorizer.log` you should find the following:

```text
DEBUG operation = Write on resource = Topic:LITERAL:ssl from host = 127.0.0.1 is Allow based on acl = User:CN=producer has Allow permission for operations: Write from hosts: * (kafka.authorizer.logger)
DEBUG Principal = User:CN=producer is Allowed Operation = Describe from host = 127.0.0.1 on resource = Topic:LITERAL:ssl for request = Metadata with resourceRefCount = 1 (kafka.authorizer.logger)
DEBUG operation = Write on resource = Topic:LITERAL:ssl from host = 127.0.0.1 is Allow based on acl = User:CN=producer has Allow permission for operations: Write from hosts: * (kafka.authorizer.logger)
DEBUG Principal = User:CN=producer is Allowed Operation = Write from host = 127.0.0.1 on resource = Topic:LITERAL:ssl for request = Produce with resourceRefCount = 1 (kafka.authorizer.logger)
```

## Consume Messages

Create `consumer.properties` file as a minimal configuration of a Kafka client to use SSL authentication and identify itself as `CN=consumer`:

```text
security.protocol=SSL
ssl.truststore.location=/tmp/kafka-ssl-demo/client.truststore
ssl.truststore.password=123456
ssl.keystore.location=/tmp/kafka-ssl-demo/consumer.keystore
ssl.keystore.password=123456
ssl.key.password=123456
```

Use `kafka-console-consumer.sh` utility to consume messages as `CN=consumer`:

```shell
kafka-console-consumer.sh \
  --bootstrap-server :9093 \
  --topic ssl \
  --consumer.config /tmp/kafka-ssl-demo/consumer.properties
```

In `logs/kafka-authorizer.log` you should find the following:

```text
DEBUG operation = Read on resource = Topic:LITERAL:ssl from host = 127.0.0.1 is Allow based on acl = User:CN=consumer has Allow permission for operations: Read from hosts: * (kafka.authorizer.logger)
DEBUG Principal = User:CN=consumer is Allowed Operation = Read from host = 127.0.0.1 on resource = Topic:LITERAL:ssl for request = Fetch with resourceRefCount = 1 (kafka.authorizer.logger)
```

*That's all for the demo. Thanks for reading!*
