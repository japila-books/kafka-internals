# Demo: Secure Inter-Broker Communication

The demo shows how to set up a secure communication between brokers (and disable the unsecure plaintext listener altogether). That will make Kafka brokers available via TLS/SSL only.

## Before You Begin

The demo is a follow-up to [Demo: Securing Communication Between Clients and Brokers Using SSL](securing-communication-between-clients-and-brokers.md). Please finish it first before this demo.

## Configure Broker to Trust Certificate Authority

Import the certificate of the certificate authority (CA) to a broker truststore so the brokers can trust it (when a broker tries to connect using SSL).

```shell
$ keytool \
  -import \
  -file ca.crt \
  -keystore server.truststore \
  -alias ca \
  -storepass 123456 \
  -noprompt
Certificate was added to keystore
```

Use `keytool` to print out the certificates in the client keystore.

```shell
keytool -list -v -keystore server.truststore -storepass 123456
```

There should be 1 entry for the CA.

```shell
$ keytool -list -v -keystore server.truststore -storepass 123456
Keystore type: PKCS12
Keystore provider: SUN

Your keystore contains 1 entry

Alias name: ca
# ...removed for brevity
```

## Enable SSL for Inter-Broker Communication

Edit `config/server-ssl.properties` and add the following configuration properties to enable SSL for inter-broker communication:

```text
security.inter.broker.protocol=SSL
ssl.truststore.location=/tmp/kafka-ssl-demo/server.truststore
ssl.truststore.password=123456
```

Start the broker(s).

```shell
./bin/kafka-server-start.sh config/server-ssl.properties
```

!!! tip
    Use `export KAFKA_OPTS=-Djavax.net.debug=all` to debug SSL issues.

Verify the SSL configuration of the broker. The following uses the Cryptography and SSL/TLS Toolkit (OpenSSL) and the client tool.

```shell
openssl s_client -connect localhost:9093
```

## Disable Plaintext Unsecure Listener

Edit `config/server-ssl.properties` and change `listeners` property to use `SSL://:9093` only:

```text
listeners=SSL://:9093
```

Start the broker(s).

```shell
./bin/kafka-server-start.sh config/server-ssl.properties
```

!!! tip
    Use `export KAFKA_OPTS=-Djavax.net.debug=all` to debug SSL-related issues.

Verify the SSL configuration of the broker. The following uses the Cryptography and SSL/TLS Toolkit (OpenSSL) and the client tool.

```shell
openssl s_client -connect localhost:9093
```

Enter `Ctrl-C` to close the session.

*That's all for the demo. I hope you enjoyed it!*
