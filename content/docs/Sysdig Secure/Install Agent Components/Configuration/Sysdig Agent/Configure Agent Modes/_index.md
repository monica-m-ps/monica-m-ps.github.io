---
linkTitle: "Configure Agent Modes"
title: "Configure Agent Modes"
weight: "12"
aliases:
  - /en/docs/installation/sysdig-agent/agent-configuration/configure-agent-modes/
  - /en/configure-agent-modes.html
  - /en/configure-agent-modes
  - /en/docs/installation/configuration/sysdig-agent/configure-agent-modes/
description: "Agent modes provide the ability to control metric collection to fit your scale and specific requirements. Using the appropriate mode for a specific environment helps reduce the amount of resources (CPU and memory) that the agent consumes and the number of metrics that the agent collects."
---

You can choose one of the following modes to do so. Review the features available in different agent modes before determining the mode you want to opt for.

| **Features**                                                     | <p align="center">**Monitor Mode**</P> | <p align="center">**Secure Mode**</P> | <p align="center">**Secure Light Mode**</P> | <p align="center">**Monitor with</br>Secure License**</P> |
| :----------------------------------------------------------- | :-------------------------------- | :------------------------------- | :------------------------------------- | :-------------------------------------------------------- |
| [**Runtime Policy**](/en/threat-policy)                      |                                   | <p align="center">X</p>          | <p align="center">X</p>                | <p align="center">X</p>                                   |
| [**Activity Audit**](/en/activity-audit)                     |                                   | <p align="center">X</p>          | <p align="center">X</p>                | <p align="center">X</p>                                   |
| **Captures**<br> [Monitor](/en/captures-monitor)<br> [Secure](/en/capture) | <p align="center">X</p>           | <p align="center">X</p>          | <p align="center">X</p>                | <p align="center">X</p>                                   |
| **[Network <br>Topology](/en/network)**                      |                                   | <p align="center">X</p>          |                                        | <p align="center">X</p>                                   |
| [**Prometheus**](/en/integration-library)                    | <p align="center">X</p>           |                                  |                                        | <p align="center">X</p>                                   |
| [**App Checks**](/en/integrate-applications-default-app-checks) | <p align="center">X</p>           |                                  |                                        | <p align="center">X</p>                                   |
| [**JMX**](/en/integrate-jmx-metrics-from-java-virtual-machines) | <p align="center">X</p>           |                                  | <p align="center">X</p>             | <p align="center">X</p>                                   |
| [**StatsD**](/en/integrate-statsd-metrics)                  | <p align="center">X</p>           |                                  |                                        | <p align="center">X</p>                                   |
| **Live Logs**<br> [Monitor](/en/docs/sysdig-monitor/advisor/#live-logs)<br> [Secure](/en/docs/sysdig-secure/insights/secure-live/) <br>The Live Logs feature is not accessible for Secure-only users at the moment. This functionality is available in the Sysdig Platform as a part of the Sysdig Secure offering.| <p align="center">X</p>           | <p align="center">X</p>          | <p align="center">X</p>                | <p align="center">X</p>                                   |

## Enable Agent Modes

If you have a Platform license (Sysdig Monitor and Sysdig Secure), no configuration is required. You get all the features by default. If you are opting a subset of features, determine the agent mode corresponding to your requirement.

To enable the mode you have selected, add the corresponding configuration to the `values.yaml` or `dragent.yaml` file, and restart the agent. The following sections provide configuration snippets corresponding to each agent mode.

### Monitor

The Monitor mode offers an extensive collection of metrics. We recommend this mode to monitor enterprise environments.

`monitor` is the default mode if you are running the [Enterprise tier](/en/docs/administration/administration-settings/subscription/#subscription).


#### dragent.yaml

```yaml
commandlines_capture:
  enabled: false
drift_control:
  enabled: false
drift_killer:
  enabled: false
falcobaseline:
  enabled: false
feature:
  mode: monitor
memdump:
  enabled: false
network_topology:
  enabled: false
secure_audit_streams:
  enabled: false
security:
  enabled: false
  k8s_audit_server_enabled: false
```

#### Helm

```yaml
agent:
  monitor:
    enabled: true
  secure:
    enabled: false
```

### Monitor Light

Monitor Light caters to the users that run agents in a resource-restrictive environment, or to those who are interested only in a limited set of metrics.

This mode provides CPU, Memory, File, File system, and Network metrics. For more information, see [Metrics Available in Monitor Light](/en/metrics-available-in-monitor-light).


#### dragent.yaml

```yaml
app_checks_enabled: false
drift_control:
  enabled: false
drift_killer:
  enabled: false
falcobaseline:
  enabled: false
feature:
  mode: monitor_light
jmx:
  enabled: false
memdump:
  enabled: false
network_topology:
  enabled: false
prometheus:
  enabled: false
statsd:
  enabled: false
```

#### Helm

```yaml
agent:
  monitor:
    enabled: true
  secure:
    enabled: false
  sysdig:
    settings:
      feature:
        mode: monitor_light
```

### Secure

The secure mode supports only [Sysdig Secure](/en/docs/sysdig-secure/#sysdig-secure) features.

Sysdig agent collects no metrics in the secure mode, which, in turn, minimizes network consumption and storage requirement in the Sysdig backend. Lower resource usage can help reduce costs and improve performance.

In the Secure mode, the Monitor UI shows no data because no metrics are sent to the collector.

This feature requires agent v10.5.0 or above.


#### dragent.yaml

```yaml
app_checks_enabled: false
feature:
  mode: secure
jmx:
  enabled: false
prometheus:
  enabled: false
statsd:
  enabled: false

```

#### Helm

```yaml
agent:
  monitor:
    enabled: false
  secure:
    enabled: true
  sysdig:
    settings:
      feature:
        mode: secure
```

## Secure Light

The secure light mode supports only the following [Sysdig Secure](/en/docs/sysdig-secure/#sysdig-secure) features:

- [Runtime Policies](/en/docs/sysdig-secure/policies/)
- [Activity Audit](/en/activity-audit) 
- [Captures](/en/docs/sysdig-secure/investigate/captures/)

Sysdig agent running in `secure_light` mode consumes fewer resources than that of running in the secure mode.

This feature requires agent v12.10.0 or above.

**Note**: Do not enable Monitor features in Secure Light. If you do, you might encounter configuration errors.

#### dragent.yaml

```yaml
app_checks_enabled: false
feature:
  mode: secure_light
jmx:
  enabled: false
memdump:
  enabled: false
network_topology:
  enabled: false
prometheus:
  enabled: false
statsd:
  enabled: false
```
**Note**: `falcobaseline.enabled` should be set to `false` in agents v12.18.0 and below.

#### Helm

```yaml
agent:
  monitor:
    enabled: false
  secure:
    enabled: true
  sysdig:
    settings:
      feature:
        mode: secure_light

```

### Troubleshooting

Troubleshooting mode offers sophisticated metrics with detailed diagnostic capabilities. Some of these metrics are heuristic in nature.

In addition to the extensive metrics available in the Monitor mode, Troubleshooting mode provides additional metrics such as `net.sql` and additional segmentation for file and network metrics. For more information, see [Additional Metrics Values Available in Troubleshooting](/en/additional-metrics-values-available-in-troubleshooting).


#### dragent.yaml

 If your account is enabled for Secure, add the following:

 ```yaml
 app_checks_enabled: false
 drift_control:
   enabled: false
 drift_killer:
   enabled: false
 falcobaseline:
   enabled: false
 feature:
   mode: troubleshooting
 jmx:
   enabled: false
 memdump:
   enabled: false
 network_topology:
   enabled: false
 prometheus:
   enabled: false
 statsd:
   enabled: false
 ```

If your account is enabled for Monitor, add the following:




#### Helm

If your account is enabled for Monitor, add the following:

```yaml
agent:
  monitor:
    enabled: true
  secure:
    enabled: false
  sysdig:
    settings:
      feature:
        mode: troubleshooting
```

If your account is enabled for Secure, add the following:

 ```yaml
 agent:
   monitor:
     enabled: false
   secure:
     enabled: true
   sysdig:
     settings:
       feature:
         mode: troubleshooting
 ```


## Custom Metrics Only Mode

Custom Metrics Only mode collects the same metrics as the Monitor
Light mode, but also adds the ability to collect the following:

- Custom Metrics: StatsD, JMX, App Checks, and Prometheus
- Kubernetes State Metrics

As such, Custom Metrics Only mode is suitable if would like to use most of the
features of Monitor mode but are limited in resources.

This mode is not compatible with Secure. If your account is
configured for Secure, you must explicitly disable Secure in
the agent configuration if you wish to use this mode.

This mode requires agent v12.4.0 or above.

### Enable Custom Metrics Only Mode

1. Open the `dragent.yaml` file.

2. Add the following configuration parameter:

    ```yaml
    feature:
      mode: custom-metrics-only
    ```

3. If your account is enabled for Secure, add the following:

    ```yaml
    security:
      enabled: false
    secure_audit_streams:
      enabled: false
    falcobaseline:
      enabled: false
    ```

    This configuration explicitly disables the Secure features in the agent. If you do not disable Secure, the agent will not start due to incompatiblity issues.

4. Restart the agent.
