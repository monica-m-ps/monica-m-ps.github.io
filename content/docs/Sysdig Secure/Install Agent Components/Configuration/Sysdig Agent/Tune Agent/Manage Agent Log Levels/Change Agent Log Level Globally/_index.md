---
linkTitle: "Change Agent Log Level Globally"
title: "Change Agent Log Level Globally"
weight: "10"
aliases:
  - /en/change-agent-log-level-globally.html
  - /en/change-agent-log-level-globally
  - /en/docs/installation/sysdig-agent/agent-configuration/tune-agent/manage-agent-log-levels/change-agent-log-level-globally/
  - /en/docs/installation/sysdig-agent/agent-configuration/manage-agent-log-levels/change-agent-log-level-globally/
  - /en/docs/installation/configuration/sysdig-agent/tune-agent/manage-agent-log-levels/change-agent-log-level-globally/
---

The Sysdig agent generates log entries in `/opt/draios/logs/draios.log`.
The agent will rotate the log file when it reaches 10MB in size, keeping
the 10 most recent log files archived with a date-stamp appended to the
filename.

In order of increasing detail, the log levels available are: \[ **none
\| critical\| error \| warning \|notice \| info \| debug \| trace** \].

The default level (**info**) creates an entry for each aggregated
metrics transmission to the backend servers, once per second, in
addition to entries for any warnings and errors.

{{% callout type="warning" %}}

Setting the value lower than `info` may prohibit troubleshooting
agent-related issues.

{{% /callout %}}

The type and amount of logging can be changed by adding parameters and
log level arguments shown below to the agent's user settings
configuration file here:

`/opt/draios/etc/dragent.yaml`

After editing the `dragent.yaml` file, restart the agent at the shell
with: `service dragent restart` to affect changes.

Note that `dragent.yaml` code can be written in both YAML and JSON. The
examples below use YAML.

## File Log Level

When troubleshooting agent behavior, increase the logging to debug for
full detail:

```yaml
log:
  file_priority: debug
```

If you wish to reduce log messages going to the
`/opt/draios/logs/draios.log` file, add the `log:` parameter with one of
the following arguments under it and indented two spaces: \[ none \|
error \| warning \| info \| debug \| trace \]

```yaml
log:
  file_priority: error
```

## Container Console Logging

If you are running the containerized agent, you can also reduce
container console output by adding the additional parameter
`console_priority:` with the same arguments \[ none \| error \| warning
\| info \| debug \| trace \]

```yaml
log:
  console_priority: warning
```

Note that troubleshooting a host with less than the default 'info' level
will be more difficult or not possible. You should revert to 'info' when
you are done troubleshooting the agent.

A level of 'error' will generate the fewest log entries, a level of
'trace' will give the most, 'info' is the default if no entry exists.

## Examples 

{{% callout type="note" %}}

When using Helm charts and passing arguments either using the "--set" flags or the values files, the `logPriority` parameter allows to directly set both Agent console and file logging priorities. The possible values are "info" and "debug". This parameter is mutually exclusive with `sysdig.settings.log`, therefore they should not be used together.

{{% /callout %}}

### Using HELM

```yaml

helm install ... \
  --set agent.sysdig.settings.log.file_priority=debug \
  --set agent.sysdig.settings.log.console_priority=debug \
  sysdig/sysdig-deploy
```

OR

```yaml

helm install ... \
  --set agent.logPriority=debug \
  sysdig/sysdig-deploy
```

### Using values.yaml

```yaml
agent:
  sysdig:
    settings:
      log:
        file_priority: debug
        console_priority: debug
```

OR

```yaml
agent:
  logPriority: debug
```

### Using dragent.yaml

```yaml
customerid: 831f3-Your-Access-Key-9401
tags: local:sf,acct:eng,svc:websvr
log:
 file_priority: warning
 console_priority: info
```

OR

```yaml
customerid: 831f3-Your-Access-Key-9401
tags: local:sf,acct:eng,svc:websvr
log: { file_priority: debug, console_priority: debug }
```

### Using Docker Run Command

If you are using the "ADDITIONAL\_CONF" parameter to start a Docker
containerized agent, you would specify this entry in the Docker run
command:

```yaml
-e ADDITIONAL_CONF="log:  { file_priority: error, console_priority: none }"
-e ADDITIONAL_CONF="log:\n  file_priority: error\n  console_priority: none"
```

### Using deamonset.yaml in Kubernetes Infrastructure

When running in a Kubernetes infrastructure (installed using the v1
method, comment in the "ADDITIONAL\_CONF" line in the agent
`sysdig-daemonset.yaml` manifest file, and modify as needed:

```yaml
- name: ADDITIONAL_CONF #OPTIONAL pass additional parameters to the agent
  value: "log:\n file_priority: debug\n console_priority: error"

