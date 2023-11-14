# SharedServer

## Creating Instance

`SharedServer` takes the following to be created:

* <span id="sharedServerConfig"> [KafkaConfig](../KafkaConfig.md)
* <span id="metaProps"> [MetaProperties](MetaProperties.md)
* <span id="time"> `Time`
* <span id="_metrics"> [Metrics](../metrics/Metrics.md)
* <span id="controllerQuorumVotersFuture"> Controller Quorum Voters (`CompletableFuture[util.Map[Integer, AddressSpec]]`)
* <span id="faultHandlerFactory"> `FaultHandlerFactory`

`SharedServer` is created when:

* `KafkaRaftServer` is [created](KafkaRaftServer.md#sharedServer)
