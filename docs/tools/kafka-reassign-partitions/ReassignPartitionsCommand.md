# ReassignPartitionsCommand

`ReassignPartitionsCommand` is a [command-line application](#main) for [kafka-reassign-partitions](index.md) to [generate](#generate), [execute](#execute) and [verify](#verify) a custom partition (re)assignment configuration (as specified using a [reassignment JSON file](#reassignment-json-file)).

## Actions

### <span id="executeAssignment"><span id="execute"> execute

Executes the reassignment as specified by the [reassignment-json-file](#reassignment-json-file) option

### <span id="generateAssignment"><span id="generate"> generate

Generates a candidate partition reassignment configuration

!!! note
    This only generates a candidate assignment and does not execute it.

### <span id="verifyAssignment"><span id="verify"> verify

Verifies if the reassignment completed as specified by the [reassignment-json-file](#reassignment-json-file). If there is a throttle engaged for the replicas specified, and the rebalance has completed, the throttle will be removed

## Options

### reassignment-json-file

A JSON file with a custom partition (re)assignment configuration

The format to use is as follows:

```json
{
  "partitions": [
    {
      "topic": "foo",
      "partition": 1,
      "replicas": [
        1,
        2,
        3
      ],
      "log_dirs": [
        "dir1",
        "dir2",
        "dir3"
      ]
    }
  ],
  "version": 1
}
```

Note that `log_dirs` is optional. When specified, its length must equal the length of the replicas list. The value in this list can be either `"any"` or the absolute path of the log directory on the broker.

If absolute log directory path is specified, it is currently required that the replica has not already been created on that broker. The replica will then be created in the specified log directory on the broker later.

### replica-alter-log-dirs-throttle

The movement of replicas between log directories on the same broker will be throttled to this value (bytes/sec).

Default: `-1`

Rerunning with this option, whilst a rebalance is in progress, will alter the throttle value. The throttle rate should be at least 1 KB/s.

### throttle

The movement of partitions between brokers will be throttled to this value (bytes/sec).

Default: `-1`

Rerunning with this option, whilst a rebalance is in progress, will alter the throttle value. The throttle rate should be at least 1 KB/s.

### timeout

The maximum time (in ms) allowed to wait for partition reassignment execution to be successfully initiated

Default: `10000`

## <span id="main"> Executing Application
