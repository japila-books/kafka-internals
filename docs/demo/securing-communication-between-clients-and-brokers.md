# Demo: Securing Communication Between Clients and Brokers Using SSL

The demo shows how to configure secure communication between Kafka clients (consumers and producers) and brokers using [Transport Layer Security (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) (and its now-deprecated predecessor, Secure Sockets Layer (SSL)).

!!! note
    TLS is a newer variant of SSL, but because of the term `SSL` is used in many Kafka configuration properties the document will use `SSL` (over `TLS`) to refer to the protocol used for secure communication.

## Create Directory of SSL Files

Create a directory for SSL files.

```shell
mkdir -p /tmp/kafka-ssl-demo
```

All the following commands are executed in the directory.

```shell
cd /tmp/kafka-ssl-demo
```

## Create Certificate Authority (CA)

!!! quote "Certificate Authority"
    From [Wikipedia](https://en.wikipedia.org/wiki/Certificate_authority):

    In cryptography, a **certificate authority** or **certification authority (CA)** is an entity that issues digital certificates.

    A digital certificate certifies the ownership of a public key by the named subject of the certificate.

    A CA acts as a trusted third party — trusted both by the subject (owner) of the certificate and by the party relying upon the certificate. The format of these certificates is specified by the X.509 standard.

Simply put, a certificate authority is used to sign certificates (with public keys).

Generate a private key and a self-signed certificate for the CA.

```shell
// openssl genrsa -out ca.key
// openssl req -new -x509 -key ca.key -out ca.crt
$ openssl req \
  -new \
  -x509 \
  -days 365 \
  -keyout ca.key \
  -out ca.crt \
  -subj "/C=PL/L=Warsaw/CN=Certificate Authority" \
  -passout pass:1234
Generating a RSA private key
............................+++++
.......................+++++
writing new private key to 'ca.key'
-----
```

You should have the following files in the directory:

* `ca.key` - the private key of the certificate authority
* `ca.crt` - the certificate (with the public key) of the certificate authority

```shell
$ tree
.
├── ca.crt
└── ca.key
```

Use the following command to print certificate contents:

```shell
$ openssl x509 -text -noout -in ca.crt
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            5f:a1:fb:63:86:36:d1:e4:c3:f4:46:93:2e:0f:f8:34:26:6c:79:39
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: C = PL, L = Warsaw, CN = Certificate Authority
        Subject: C = PL, L = Warsaw, CN = Certificate Authority
# ... removed for clarity
```

Since `ca.crt` is a self-signed certificate, the `Issuer` and `Subject` sections use the same subject.

## Generate SSL Keys and Certificate for Kafka Brokers

Generate the keys and certificate for every Kafka broker in a cluster. This time you'll be using `keytool` utility that comes with Java Platform, Standard Edition (refer to [keytool](https://docs.oracle.com/en/java/javase/11/tools/keytool.html)).

```shell
// openssl genrsa -out server.key
$ keytool \
  -genkey \
  -keystore server.keystore \
  -alias localhost \
  -dname CN=localhost \
  -keyalg RSA \
  -validity 365 \
  -ext san=dns:localhost \
  -storepass 123456
```

!!! tip
    If you want to use a Subject Alternative Name (SAN) in your certificate, add the following options to the `keytool` command line:

    * Fully-qualified domain name (FQDN): `–ext san=dns:servername.domain.com`
    * IP address: `–ext san=ip:192.168.10.1`

    **DistinguishedName** or **SubjectAltNames** should be the fully-qualified domain name of a broker.

You should now have one more file in the directory:

* `server.keystore` - the keystore with the private key and the certificate of the broker

```text
$ tree
.
├── ca.crt
├── ca.key
└── server.keystore
```

Use `keytool` to print out the content of the keystore.

```shell
keytool -list -v -keystore server.keystore -storepass 123456
```

The keystore should contain 1 entry for the alias `localhost`.

```shell
$ keytool -list -v -keystore server.keystore -storepass 123456
Keystore type: PKCS12
Keystore provider: SUN

Your keystore contains 1 entry

Alias name: localhost
# ... removed for clarity
```

## Sign Broker Certificate (Using CA)

Create a certificate signing request (CSR).

Export the server certificate from `server.keystore`.

```shell
// openssl req -new -key server.key -out server.csr
$ keytool \
  -certreq \
  -keystore server.keystore \
  -alias localhost \
  -file server.unsigned.crt \
  -storepass 123456
```

You should now have one more file in the directory:

* `server.unsigned.crt` - a certificate signing request of the server certificate

```text
$ tree
.
├── ca.crt
├── ca.key
├── server.keystore
└── server.unsigned.crt
```

Sign the certificate signing request (`server.unsigned.crt`) with the root CA.

```shell
$ openssl x509 \
  -req \
  -CA ca.crt \
  -CAkey ca.key \
  -in server.unsigned.crt \
  -out server.crt \
  -days 365 \
  -CAcreateserial \
  -passin pass:1234
Signature ok
subject=CN = localhost
Getting CA Private Key
```

You should have the following files in the directory:

* `ca.srl`
* `server.crt` - the signed certificate of the broker

```text
$ tree
.
├── ca.crt
├── ca.key
├── ca.srl
├── server.crt
├── server.keystore
└── server.unsigned.crt
```

## Import Certificates to Broker Keystore

Create a SSL keystore for the Kafka broker. Each broker gets its own unique keystore.

Import the certificate of the CA into the broker keystore.

```shell
$ keytool \
  -import \
  -file ca.crt \
  -keystore server.keystore \
  -alias ca \
  -storepass 123456 \
  -noprompt
Certificate was added to keystore
```

Import the signed certificate into the broker keystore. Make sure to use the same `-alias` as you used ealier.

```shell
$ keytool \
  -import \
  -file server.crt \
  -keystore server.keystore \
  -alias localhost \
  -storepass 123456 \
  -noprompt
Certificate reply was installed in keystore
```

Use `keytool` to print out the certificates in the broker keystore.

```shell
keytool -list -v -keystore server.keystore -storepass 123456
```

There should be 2 entries (one for the CA and another for the broker itself).

```shell
$ keytool -list -v -keystore server.keystore -storepass 123456

Keystore type: PKCS12
Keystore provider: SUN

Your keystore contains 2 entries

Alias name: ca
# ... removed from clarity
Alias name: localhost
# ... removed from clarity
```

## Configure SSL on Kafka Broker

Create `config/server-ssl.properties` (based on `config/server.properties`) and add the following configuration properties to enable SSL:

```text
listeners=PLAINTEXT://:9092,SSL://:9093
ssl.keystore.location=/tmp/kafka-ssl-demo/server.keystore
ssl.keystore.password=123456
ssl.key.password=123456
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

The tool should print out the certificate chain of the broker (a chain of the subjects and the issuers). At the end, you should find the following `Verify return code`:

```text
Verify return code: 19 (self signed certificate in certificate chain)
```

Enter `Ctrl-C` to close the session.

Use the client tool with `-CAfile` option to trust the CA certificate.

```shell
openssl s_client -connect localhost:9093 -CAfile /tmp/kafka-ssl-demo/ca.crt
```

With the change, you should find the following `Verify return code`:

```text
Verify return code: 0 (ok)
```

Enter `Ctrl-C` to close the session.

## Import CA Certificate to Client Truststore

Add the CA certificate `ca.crt` to a client truststore for the clients to trust this CA.

```shell
$ keytool \
  -import \
  -file ca.crt \
  -keystore client.truststore \
  -alias ca \
  -storepass 123456 \
  -noprompt
Certificate was added to keystore
```

Use `keytool` to print out the certificates in the client keystore.

```shell
keytool -list -v -keystore client.truststore -storepass 123456
```

There should be 1 entry for the CA.

```shell
$ keytool -list -v -keystore client.truststore -storepass 123456

Keystore type: PKCS12
Keystore provider: SUN

Your keystore contains 1 entry

Alias name: ca
# ... removed for brevity
```

## Configure SSL for Kafka Clients

Create `/tmp/kafka-ssl-demo/client-ssl.properties` as a minimal configuration of a Kafka client to use SSL:

```text
security.protocol=SSL
ssl.truststore.location=/tmp/kafka-ssl-demo/client.truststore
ssl.truststore.password=123456
```

Use `kafka-console-producer.sh` utility to send records to Kafka brokers over SSL:

```shell
./bin/kafka-console-producer.sh \
  --broker-list :9093 \
  --topic ssl \
  --producer.config /tmp/kafka-ssl-demo/client-ssl.properties
```

*That's all for the demo. I hope you enjoyed it!*
