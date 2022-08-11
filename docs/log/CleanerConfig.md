# CleanerConfig

`CleanerConfig` contains the configuration parameters of the [LogCleaner](LogCleaner.md).

Parameter | Configuration Property | Default Value
----------|------------------------|--------------
 numThreads | [log.cleaner.threads](../KafkaConfig.md#logCleanerThreads) | 1
 dedupeBufferSize | [logCleanerDedupeBufferSize](../KafkaConfig.md#logCleanerDedupeBufferSize) | 4*1024*1024L
 dedupeBufferLoadFactor | [logCleanerDedupeBufferLoadFactor](../KafkaConfig.md#logCleanerDedupeBufferLoadFactor) | 0.9d
 ioBufferSize | [logCleanerIoBufferSize](../KafkaConfig.md#logCleanerIoBufferSize) | 1024*1024
 maxMessageSize | [messageMaxBytes](../KafkaConfig.md#messageMaxBytes) | 32*1024*1024
 maxIoBytesPerSecond | [logCleanerIoMaxBytesPerSecond](../KafkaConfig.md#logCleanerIoMaxBytesPerSecond) | Double.MaxValue
 backOffMs | [log.cleaner.backoff.ms](../KafkaConfig.md#logCleanerBackoffMs) | 15*1000
 enableCleaner | [log.cleaner.enable](../KafkaConfig.md#logCleanerEnable) | true
 hashAlgorithm | | MD5
