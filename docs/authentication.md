# Authentication

**Kafka Authentication** is based on the following:

* [security.protocol](clients/CommonClientConfigs.md#security.protocol) configuration property
* [ssl.client.auth](KafkaConfig.md#ssl.client.auth) configuration property

For SSL with client authentication enabled, `TransportLayer#handshake()` performs authentication. For SASL, authentication is performed by `Authenticator#authenticate()`.

For SSL authentication,  the principal will be derived using the rules defined by [ssl.principal.mapping.rules](KafkaConfig.md#ssl.principal.mapping.rules) applied on the distinguished name from the client certificate if one is provided. Otherwise, if client authentication is not required, the principal name will be `ANONYMOUS`.

For `PLAINTEXT` listeners or when client authentication is not required, the principal will always be `ANONYMOUS`.
