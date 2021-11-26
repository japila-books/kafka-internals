# Sensor

## Creating Instance

`Sensor` takes the following to be created:

* <span id="registry"> [Metrics](Metrics.md)
* <span id="name"> Name
* <span id="parents"> Parent `Sensor`s
* <span id="config"> [MetricConfig](MetricConfig.md)
* <span id="time"> `Time`
* <span id="inactiveSensorExpirationTimeSeconds"> `inactiveSensorExpirationTimeSeconds`
* <span id="recordingLevel"> `RecordingLevel`

`Sensor` is created when:

* `Metrics` is requested for a [sensor](Metrics.md#sensor)

## <span id="shouldRecord"> shouldRecord

```java
boolean shouldRecord()
```

`shouldRecord` requests the [RecordingLevel](#recordingLevel) to `shouldRecord` based on the configured [RecordingLevel](MetricConfig.md#recordLevel) (in the [MetricConfig](#config)).
