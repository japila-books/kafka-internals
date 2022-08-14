# Demo: SSL Authentication

The demo shows how to use SSL/TLS for authentication so no connection can be established between Kafka clients (consumers and producers) and brokers unless a valid and trusted certificate is provided.

## Before You Begin

The demo is a follow-up to [Demo: Secure Inter-Broker Communication](secure-inter-broker-communication.md). Please finish it first before this demo.

## Generate Certificate for Client Authentication

Generate the keys and certificate of a Kafka client to be authenticated as *jacek*.

```shell
keytool \
  -genkey \
  -keystore jacek.keystore \
  -alias jacek \
  -dname CN=jacek \
  -keyalg RSA \
  -validity 365 \
  -storepass 123456
```

You should now have one more file in the directory:

* `jacek.keystore` - the keystore with the private key and the certificate of the user

Use `keytool` to print out the content of the keystore.

```shell
keytool -list -v -keystore jacek.keystore -storepass 123456
```

The keystore should contain 1 entry for the alias `jacek`.

## Sign Client Certificate (Using CA)

Create a certificate signing request (CSR).

Export the client certificate from `jacek.keystore`.

```shell
keytool \
  -certreq \
  -keystore jacek.keystore \
  -alias jacek \
  -file jacek.unsigned.crt \
  -storepass 123456
```

Sign the certificate signing request (`jacek.unsigned.crt`) with the root CA.

```console
$ openssl x509 \
  -req \
  -CA ca.crt \
  -CAkey ca.key \
  -in jacek.unsigned.crt \
  -out jacek.crt \
  -days 365 \
  -CAcreateserial \
  -passin pass:1234
Signature ok
subject=CN = jacek
Getting CA Private Key
```

You should have the following file in the directory:

* `jacek.crt` - the signed certificate of the user

## Import Certificates to Client Keystore

Create a SSL keystore for the Kafka client. Each client gets its own unique keystore.

Import the certificate of the CA into the client keystore.

``` console
$ keytool \
  -import \
  -file ca.crt \
  -keystore jacek.keystore \
  -alias ca \
  -storepass 123456 \
  -noprompt
Certificate was added to keystore
```

Import the signed certificate into the client keystore. Make sure to use the same `-alias` as you used ealier.

``` console
$ keytool \
  -import \
  -file jacek.crt \
  -keystore jacek.keystore \
  -alias jacek \
  -storepass 123456 \
  -noprompt
Certificate reply was installed in keystore
```

Use `keytool` to print out the certificates in the client keystore.

```shell
keytool -list -v -keystore jacek.keystore -storepass 123456
```

There should be 2 entries (one for the CA and another for the client itself).

## Require Client Authorization Using SSL on Kafka Brokers

Enable SSL authentication (require client authentication using SSL certificates).

Edit `config/server-ssl.properties` and add the following configuration property:

```text
ssl.client.auth=required
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

The client tool will quit immediately since the broker requires clients to provide valid certificates. You should find the following INFO message in the broker logs:

```text
[SocketServer brokerId=0] Failed authentication with /0:0:0:0:0:0:0:1 (SSL handshake failed)
```

## Configure SSL Authentication for Kafka Client

Use the following `jacek-client.properties` as a minimal configuration of a Kafka client to use SSL authentication:

```text
security.protocol=SSL
ssl.truststore.location=/tmp/kafka-ssl-demo/client.truststore
ssl.truststore.password=123456
ssl.keystore.location=/tmp/kafka-ssl-demo/jacek.keystore
ssl.keystore.password=123456
ssl.key.password=123456
```

Use `kafka-console-producer.sh` utility to send records to Kafka brokers over SSL:

```shell
kafka-console-producer.sh \
  --broker-list :9093 \
  --topic ssl \
  --producer.config /tmp/kafka-ssl-demo/jacek-client.properties
```

!!! tip
    Use `export KAFKA_OPTS=-Djavax.net.debug=all` to debug SSL issues. Learn more in the source code of openjdk's [sun.security.ssl.SSLLogger](https://github.com/AdoptOpenJDK/openjdk-jdk11u/blob/master/src/java.base/share/classes/sun/security/ssl/SSLLogger.java).

*That's all for the demo. I hope you enjoyed it!*
