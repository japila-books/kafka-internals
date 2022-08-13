# Consumer Groups

[GroupCoordinator](GroupCoordinator.md) is the main actor here and is started alongside [KafkaServer](../broker/KafkaServer.md#groupCoordinator).

## <span id="__consumer_offsets"> __consumer_offsets

`__consumer_offsets` is the offset commit topic (with [offsets.topic.num.partitions](../KafkaConfig.md#offsets.topic.num.partitions)).
