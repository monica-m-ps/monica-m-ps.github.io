---
linkTitle: "Manage Serverless Agent Logs"
title: "Manage Serverless Agent Logs"
weight: "13"
aliases:
  - /en/ecs-manage-logs
Description: "Even if the Sysdig Workload Agent runs in the same container as the workload it instruments, their log streams are handled separately."
---
 - Workload logs remain with whatever log setup you have on your task container.
 - Instrumentation logs, namely the logs associated with the Workload Agent, are stored in a separate log group created by the serverless instrumentation stack:

  	`<stack_name>-SysdigLogGroup-<uid>`

  Currently, the log group name for instrumentation cannot be edited.

You can configure the instrumentation logs by using the environment variables described below.


## Set Global Log Level

The environment variable `SYSDIG_LOGGING` sets the global instrumentation log level.

It defaults to `info`.

Available options are: `silent` | `fatal` | `critical` | `error` | `warning` | `info` | `debug` | `trace`.


## Configure the Logging Mechanism

As of [serverless agent version 4.0.0](/en/docs/release-notes/serverless-agent-release-notes/#400-february-10-2023), you can fine-tune the Workload Agent as described in [Manage Agent Log Levels](/en/manage-agent-log-levels) as follows:

- Prevent excessive logging from certain components
- Enable extra logging from specific components for troubleshooting

Use the environment variable `SYSDIG_EXTRA_CONF` to do so.

The following example shows the configuration string sets the global log level to `info` and the log level of the `security_mgr` component to `warning`.

```
SYSDIG_EXTRA_CONF="log: {  console_priority: info,  console_priority_by_component:   [ 'security_mgr: warning' ] }"
```

## Log Forwarding

By default, the instrumentation logs are wrapped as JSON ojects and  forwarded to the `<stack_name>-SysdigLogGroup-<uid>` log group.

Configure log forwarding using the following environment variables:

| Environment Variable        | Default | Description                                                  |
| --------------------------- | ------- | ------------------------------------------------------------ |
| `SYSDIG_ENABLE_LOG_FORWARD` | `true`  | Enables and disables the log forwarding. Set to `false` to disable the log forwarding. |
| `SYSDIG_LOG_FORWARD_FORMAT` | `json`  | Defines the format of the instrumentation log events. Set to `text` to get logs in plaintext. |

### Disable Log Forwarding

Use the following configuration to disable the log forwarding. If you do so, the Workload Agent logs will be stored along with the workload logs.

```
SYSDIG_ENABLE_LOG_FORWARD="false"
```

Note that when log forwarding is disabled, the log forwarding format switches to `text` automatically.

### Forward Logs in Text Format

To forward instrumentation logs  to text format, use the following configuration string:

```
SYSDIG_LOG_FORWARD_FORMAT="text"
```

You can also configure the log Workload Agent to forward logs to a different TCP endpoint by using the following environment variables:

| Environment Variable      | Default           | Description                                                                 |
|---------------------------|-------------------|-----------------------------------------------------------------------------|
| `SYSDIG_LOG_LISTEN_PORT`  | `32000`           | The port of the log listener.                                               |
| `SYSDIG_LOG_FORWARD_ADDR` | `localhost:32000` | The TCP endpoint the logwriter of the Workload Agent listens.               |
| `SYSDIG_BUFFER_SIZE`      | `1024`            | The size in bytes of the rx buffer of the TCP listener. Default `1024`.     |
| `SYSDIG_DEADLINE_SECONDS` | `3`               | Connection deadline. You can increase it, if clients take longer to connect. |