---
linkTitle: "View Agent Health"
title: "View Agent Health"
weight: "13"
aliases:
  - /en/view-agent-health
description: "The Sysdig Agent uses ReadinessProbe to determine its readiness to accept incoming requests. Additionally, the agent can generate internal health metrics through a Prometheus exporter."
---

## Readiness Probe for Sysdig Agent

Kubernetes uses readiness or liveness probes to determine the readiness or live status of a pod.  Similar functionality is present in Sysdig Agent versions preceding v12.17.0, where the readiness state of the agent pod can be determined by checking for the existence of a file named `running` in the `/opt/draios/logs` directory. The presence of the `running` file indicates that the agent is connected both to the Sysdig backend and the Kubernetes API server.

In Sysdig Agent version 12.17.0, an HTTP port can be employed to query its health status, aligning with the convention followed by typical Kubernetes services. Beyond assessing backend and API server connectivity, this health status considers the stability and running status of all its sub-processes. In version 12.17.0, the health service defaults to listening on port 24483 across all host interface addresses.

Starting from agent version 12.19.0, the default behavior was modified to have the health service exclusively listen on the localhost (127.0.0.1) address.

To adjust the host address to listen to, specify the the following configuration in the `dragent.yaml` file.

```
status_host: 127.0.0.1
```

To change the port to listen to, add the following:

```
status_port: 24483
```

Starting from Helm chart v1.18.1, the new HTTP-based readiness endpoint is enabled by default.

If you are not using our Helm charts to install the agent, you can configure the readiness probe manually by adding the following configuration to the daemonset specification:

```yaml
readinessProbe:
    failureThreshold: 3
    httpGet:
        host: 127.0.0.1
        path: /healthz
        port: 24483
    initialDelaySeconds: 90
    periodSeconds: 5
    timeoutSeconds: 2
```

The initial delay can be customized depending on the typical time it takes for the agents to become ready in your cluster. This duration typically varies based on cluster size and the number of entities that the agents need to download initially to obtain the complete state of the cluster.


## Collect Agent Health Metrics

You can export agent metrics using the Sysdig Agent Prometheus exporter. These metrics serve as a valuable tool for troubleshooting and retrieving status information from the agent, especially in scenarios where it encounters difficulties connecting to the backend.

### Enable the Agent Prometheus Exporter

To enable the exporter using our Helm chart, set the following in your `values.yaml`:

```yaml
agent:
  sysdig:
    settings:
      prometheus_exporter:
        enabled: true
        export_health_metrics: true
```

To enable the exporter, add the following to the `dragent.yaml`:

```yaml
prometheus_exporter:
    enabled: true
```

By default the prometheus exporter will listen on port 9544 by using the standard path, `/metrics`.

To change the port or IP address to listen to, you can use the `listen_url` configuration parameter. For example:

```yaml
prometheus_exporter:
		listen_url: 127.0.0.1:9544
```

#### Enable Authentication

To enable basic authentication in Prometheus Exporter, add the following to your `values.yaml` file:


```yaml
agent:
  sysdig:
    settings:
      prometheus_exporter:
        enabled: true
        export_health_metrics: true
        basic_auth_users:
            <promuser>: <bcrypt-password-hash>
```
Replace <promuser> and <bcrypt-password-hash> with the the credentials required to connect to Prometheus. Passwords must be hashed with [bcrypt](https://github.com/prometheus/exporter-toolkit/blob/master/docs/web-configuration.md#about-bcrypt).

#### Enable TLS Authentication

To enable TLS authentication in Prometheus Exporter, add the following to your `values.yaml` file:

```yaml

agent:
  sysdig:
    settings:
      prometheus_exporter:
        enabled: true
        export_health_metrics: true
        basic_auth_users:
            <promuser>: <bcrypt-password-hash>
        tls_server_config:
            cert_file: /opt/draios/etc/kubernetes/certs/example.com/example.com.crt
            key_file: /opt/draios/etc/kubernetes/certs/example.com/example.com.key

            client_ca_file: /opt/draios/etc/kubernetes/certs/client.com/ca.pem
            client_auth_type: RequireAndVerifyClientCert
```
Replace <promuser> and <bcrypt-password-hash> with the the credentials required to connect to Prometheus. Passwords must be hashed with [bcrypt](https://github.com/prometheus/exporter-toolkit/blob/master/docs/web-configuration.md#about-bcrypt).

For more information on `basic_auth_users` and `tls_server_config`, see [web configuration](https://github.com/prometheus/exporter-toolkit/blob/master/docs/web-configuration.md).

### Agent Health Metrics

Once enabled, the agent's Prometheus exporter will automatically export the following metrics:

```
container_cpu_used_percent
container_file_time_in
container_file_time_other
container_file_time_out
container_memory_bytes_used
container_memory_swap_bytes_used
host_cpu_idle_percent
host_cpu_iowait_percent
host_cpu_nice_percent
host_cpu_stolen_percent
host_cpu_system_percent
host_cpu_used_percent
host_cpu_user_percent
host_file_time_in
host_file_time_other
host_file_time_out
host_memory_bytes_available
host_memory_bytes_total
host_memory_bytes_used
host_memory_bytes_virtual
host_memory_swap_bytes_available
host_memory_swap_bytes_total
host_memory_swap_bytes_used
promhttp_metric_handler_requests_in_flight
promhttp_metric_handler_requests_total
sysdig_sampling_ratio
sysdig_up
```

As of agent v12.19.0, you can export additional health metrics with:


```yaml
prometheus_exporter:
    enabled: true
    export_health_metrics: true
```
{{% callout type="note" %}}

You need to add the `prometheus.io/scrape: "true"` and `prometheus.io/port: 9544` annotations to the agent daemonSet to allow the Agent's Prometheus native service discovery to find the endpoint.

{{% /callout %}}

The additional metrics you can retrieve are:


```
sysdig_agent_host_info: 1.0
    host_hostname
    cluster_name
    agent_version
    agent_mode

sysdig_agent_healthy: 1/0


sysdig_agent_connected: 1/0

sysdig_agent_connection_error_code: [0-5]
    agent_error
    backend_error_code
    backend_error_msg

sysdig_agent_unlicensed: 1/0

sysdig_agent_analyzer_num_evts: <number>

sysdig_agent_analyzer_dropped_evts: <number>

host_uname: 1.0
    machine_type
    kernel_name
    kernel_release
    kernel_version

sysdig_agent_process_memory_kb: <memory used in kilo bytes >
    agent_process

sysdig_agent_feature_enabled: <Use the agent_feature label to retrieve the list of features enabled>

sysdig_agent_process_uptime_s: <Use the agent_process label to retrieve the  process uptime in seconds> 
sysdig_agent_analyzer_num_evts
sysdig_agent_analyzer_dropped_evts
```

where

`sysdig_agent_host_info` always return a value of `1`.

`sysdig_agent_healthy` returns either `1` or `0`. `1` indicates that it is healthy.

`sysdig_agent_connected` returns either `1` or `0`. `1` indicates that it is connected.

`sysdig_agent_unlicensed` returns either `1` or `0`. `1` indicates that it is unlicensed.

`sysdig_agent_feature_enabled` returns either `1` or `0`. `1` indicates that the feature is enabled. 
 
 For example:

 ```
sysdig_agent_feature_enabled{agent_feature="cointerface"} 1
sysdig_agent_feature_enabled{agent_feature="jmx"} 0
```

[0-5] represents error codes returned by `sysdig_agent_connection_error_code `:

```
0 - No Error
1 - I/O Exception
2 - Connection timed out
3 - Invalid argument
4 - Handshake error
5 - Invalid message received
6 - Error received from collector or backend
```
