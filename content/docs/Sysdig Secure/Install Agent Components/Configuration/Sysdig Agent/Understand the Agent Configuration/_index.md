---
linkTitle: "Configure the Agent"
title: "Configure the Agent"
weight: "10"
aliases:
  - /en/understand-the-agent-configuration.html
  - /en/understand-the-agent-configuration
  - /en/docs/installation/sysdig-agent/agent-configuration/understand-the-agent-configuration/
  - /en/docs/installation/configuration/sysdig-agent/understand-the-agent-configuration/
  - /en/configure-agent/
description: "Out of the box, the Sysdig agent gathers and reports on a wide variety of predefined metrics. To collect additional metrics, configure the agent."
---

The file `dragent.yaml` defines metrics collection parameters. Modify `dragent.yaml` to configure the agent. How you configure `dragent.yaml` depends on whether the agent was installed:

- In a Kubernetes environment.
- In a non-orchestrated container, such as a Docker.
- As a Linux package.

Follow the instructions on this page to implement configurations found in the [Configuration Library](/en/configuration-library).

## Kubernetes

If Sysdig agent is installed in a Kubernetes environment with Helm, you can edit the `dragent.yaml` with Helm.

To edit `dragent.yaml` with Helm, you can:
- Add configuration to `values.yaml`.
- Use key-values as inline arguments with `helm install`.

For example, to edit `dragent.yaml` in Helm syntax:

```bash
helm install sysdig-agent \
  --namespace sysdig-agent \
  --set global.clusterConfig.name='my_cluster' \
  --set global.sysdig.tags.{tag_name_1}={tag_value_1} \
  --set global.sysdig.tags.{tag_name_2}={tag_value_2} \
  --set global.sysdig.tags.{tag_name_3}={tag_value_3} \
  sysdig/sysdig-deploy
```
where for each `tag_name` you have a specific `tag_value` like:

```bash
helm install sysdig-agent \
  --namespace sysdig-agent \
  --set global.clusterConfig.name='my_cluster' \
  --set global.sysdig.tags.linux=ubuntu \
  --set global.sysdig.tags.dept=dev \
  --set global.sysdig.tags.local=nyc \
  sysdig/sysdig-deploy

```

This command will be translated into the following:

```yaml
data:
  dragent.yaml: |
    tags: linux:ubuntu,dept:dev,local:nyc
    k8s_cluster_name: my_cluster
```

For more details, including instruction on utilizing `values.yaml` see [Sysdig Deploy](https://charts.sysdig.com/charts/sysdig-deploy/).

## Container

If Sysdig agent is installed in a non-orchestrated environment such as Docker, you can edit the `dragent.yaml` file in one or more of the following ways:

- Mount the `dragent.yaml` file as a Docker volume inside the container.

  ```bash
  docker run -v /home/admin-user/config-files/sysdig-agent/dragent.yaml:/opt/draios/etc/dragent.yaml ... quay.io/sysdig/agent
  ```
- Pass parameters that will be appended to a dynamically generated `dragent.yaml` file via the `ADDITIONAL_CONF` environment variable.
  ```bash
  docker run -e ADDITIONAL_CONF="<dragent.yaml parameters>" ... quay.io/sysdig/agent
  ```
  If `dragent.yaml` is mounted as a Docker volume inside the container, the `ADDITIONAL_CONF` environment variable will be ignored.
- Use environment variables such as `COLLECTOR`, `ACCESS_KEY`, `TAGS`, and so on to add or override specific parameters in `dragent.yaml`.
- Pass environment variables directly to the agent such as `SYSDIG_AGENT_DRIVER` or `SYSDIG_BPF_PROBE`.


### Edit dragent.yaml

1. Mount `dragent.yaml` as a container.

2. Log in to the host where the agent is installed.

3. Locate and open `dragent.yaml`.

    If `dragent.yaml` is mounted inside an agent container as a Docker volume, it may be located anywhere on the host that the administrator finds convenient.

4. Edit the file using proper YAML syntax. See [Examples](#examples).

5. For changes to take effect, restart the agent with the command: 

   ```
   docker restart sysdig-agent
   ```

### docker run 

Use the `docker run` command with `-e ADDITIONAL_CONF="<VARIABLES>"` where `<VARIABLES>` contains all the customized parameters you want to include.

#### Convert YAML Parameters to Single-Line Format

To insert `ADDITIONAL_CONF` parameters in a `docker run` command or a DaemonSet file, you must convert the YAML code into a single line. You can do the conversion manually for short snippets. To convert longer portions of YAML, use `echo|sed` commands:

1.  Write your configuration in YAML, as it would be entered directly in `dragent.yaml`.
    
2.  In a Bash shell, use `echo` and `sed` to convert to a single line:
    
    ```bash
    echo '<YAML_CONTENT>' | sed -e ':a' -e 'N' -e '$!ba' -e 's/\n/\\n/g'
    ```
    
3. Insert the resulting line into the `docker run` command or add it to the DaemonSet file as an `ADDITIONAL_CONF`.


## Linux

If the Sysdig agent is installed in a Linux host via a `.rpm` or `.deb` package, edit `dragent.yaml` directly.

- On `.rpm` installations, environment variables may be specified in `/etc/sysconfig/dragent`. 

- On `.deb` installations, environment variables may be specified in `/etc/default/dragent`. 

The [systemd](https://systemd.io/) supervisor does not support inline comments for environment variables. If you edit the file after setup, do not write comments on the same line where you define the environment variable.

The agent and its probe-loader shell script understand the following environment variables:
* `SYSDIG_AGENT_DRIVER` (12.17.0 and newer)
* `SYSDIG_BPF_PROBE`

Use one of the following:

- Agent version 12.17.0 or newer
  ```bash
  SYSDIG_AGENT_DRIVER=universal_ebpf
  ```
- Agent versions before 12.17.0
  ```bash
  export SYSDIG_BPF_PROBE=""
  ```

This environment file is sourced directly by the agent init script.  For agent versions before  12.17.0, the `export` keyword is required.

### Edit dragent.yaml 

1. Log in to the host where the agent is installed.

2. Open `/opt/draios/etc/dragent.yaml`.

3. Edit the file using proper YAML syntax. See [Examples](#examples).

4. For changes to take effect, restart the agent with the command:

   ```
   service dragent restart
   ```

## Examples

### Disable StatsD Collection

This example shows how to turn off StatsD collection and blacklist port 6443.

Sysdig agent uses port 6443 for both inbound and outbound communication with the Sysdig backend. The agent initiates a request and keeps a connection open with the Sysdig backend for the backend to push configurations, Falco rules, policies, and so on.

Ensure that you allow the agents' inbound and outbound communication on TCP 6443 from the respective IP addresses associated with your [SaaS Regions](/en/docs/administration/saas-regions-and-ip-ranges/). Note that you are allowing the agent to send communication outbound on TCP 6443 to the inbound IP ranges listed in the [SaaS Regions](/en/docs/administration/saas-regions-and-ip-ranges/).

#### YAML Format

```yaml
statsd:
    enabled: false
    blacklisted_ports:
    - 6443
```

#### Single-Line Format

Use spaces, hyphens, and `\n` correctly when manually converting to a single line:

`ADDITIONAL_CONF="statsd:\n    enabled: false\n    blacklisted_ports:\n    - 6443"`

You can run a full agent startup Docker command in a single line as follows:

```bash
docker run
  --name sysdig-agent \
  --privileged \
  --net host \
  --pid host \
  -e ACCESS_KEY=<ACCESS_KEY> \
  -e COLLECTOR=<COLLECTOR_ADDRESS> \
  -e TAGS=dept:sales,local:NYC \
  -e ADDITIONAL_CONF="statsd:\n    enabled: false\n    blacklisted_ports:\n    - 6443" \
  -v /var/run/docker.sock:/host/var/run/docker.sock \
  -v /dev:/host/dev \
  -v /proc:/host/proc:ro \
  -v /boot:/host/boot:ro \
  -v /lib/modules:/host/lib/modules:ro \
  -v /usr:/host/usr:ro \
  quay.io/sysdig/agent
```

### Add RabbitMQ App Check

This example helps you override the default configuration for a RabbitMQ app check.

#### YAML Format

```yaml
app_checks:
  - name: rabbitmq
    pattern:
      port: 15672
    conf:
      rabbitmq_api_url: "http://localhost:15672/api/"
      rabbitmq_user: myuser
      rabbitmq_pass: mypassword
      queues:
        - MyQueue1
        - MyQueue2
```

#### Single-Line Format (echo | sed)

From a Bash shell, issue the `echo` command and sed script.

```bash
echo "app_checks:
  - name: rabbitmq
    pattern:
      port: 15672
    conf:
      rabbitmq_api_url: "http://localhost:15672/api/"
      rabbitmq_user: myuser
      rabbitmq_pass: mypassword
      queues:
        - MyQueue1
        - MyQueue2
" | sed -e ':a' -e 'N' -e '$!ba' -e 's/\n/\\n/g'
```

This results in the single-line format to be used with `ADDITIONAL_CONF` in a Docker command or DaemonSet file.

```
"app_checks:\n - name: rabbitmq\n  pattern:\n    port: 15672\n  conf:\n    rabbitmq_api_url: http://localhost:15672/api/\n    rabbitmq_user: myuser\n    rabbitmq_pass: mypassword\n    queues:\n      - MyQueue1\n      - MyQueue2\n"
```

## Environment Variables Used by Entry Point Script for Non-Orchestrated Containers

<table>
<thead>
<tr class="header">
<th><p>Name</p></th>
<th><p>Value</p></th>
<th><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr class="even">
<td><p><code>ACCESS_KEY</code></p></td>
<td><p>Your Sysdig access key.</p></td>
<td><p>Required.</p></td>
</tr>
<tr class="odd">
<td><p><code>TAGS</code></p></td>
<td><p>Meaningful tags you want applied to your instances.</p></td>
<td><p>Optional. </p>
<p>For example:</p>
<p><code>tags: linux:ubuntu,dept:dev,local:nyc</code></p>
<p>See <a href="https://github.com/draios/sysdig-cloud-scripts/blob/master/agent_deploy/kubernetes/sysdig-agent-configmap.yaml">sysdig-agent-configmap.yaml</a>.</p></td>
</tr>
<tr class="even">
<td><p><code>REGION</code></p></td>
<td><p>The region associated with your Sysdig SaaS application.</p></td>
<td><p>Enter the <a href="/en/docs/administration/saas-regions-and-ip-ranges/">SaaS region</a>.</p>
</td>
</tr>    
<tr class="odd">
<td><p><code>COLLECTOR</code></p></td>
<td><p><code>&lt;collector-hostname.com&gt;</code></p></td>
<td><p>Enter the hostname or IP address of the Sysdig collector service. Note that when used within <code>dragent.yaml</code>, it must be lowercase (<code>collector</code>).</p>
<p>For SaaS regions, see <a href="/en/docs/administration/saas-regions-and-ip-ranges/">SaaS Regions and IP Ranges</a>.</p><p>For SaaS applications, you must use either `REGION` or `COLLECTOR`.</p></td>
</tr>
<tr class="even">
<td><p><code>COLLECTOR_PORT</code></p></td>
<td><p><code>6443</code></p></td>
<td><p>On-prem only. The port used by the Sysdig collector service. Default: <code>6443</code>.</p></td>
</tr>
<tr class="odd">
<td><p><code>SECURE</code></p></td>
<td><p><code>true</code></p></td>
<td><p>Use SSL/TLS to connect to collector service, defaults to <code>true</code>.  Set to <code>false</code> to use plaintext HTTP to communicate with the collector service, (not recommended).</p></td>
</tr>
<tr class="even">
<td><p><code>CHECK_CERTIFICATE</code></p></td>
<td><p><code>true</code></p></td>
<td><p>On-prem only. Set to <code>true</code> when using SSL/TLS to connect to the collector service and should check for a valid SSL/TLS certificate.</p></td>
</tr>
<tr class="odd">
<td><p><code>ADDITIONAL_CONF</code></p></td>
<td></td>
<td><p>Optional. A place to provide custom configuration values to the agent as environment variables.  If `dragent.yaml` is mounted as a Docker volume inside the container, `ADDITIONAL_CONF` will be ignored.</p></td>
</tr>
<tr class="even">
<td><p><code>SYSDIG_PROBE_URL</code></p></td>
<td></td>
<td><p>Optional. An alternative URL to download precompiled kernel modules.</p></td>
</tr>
</tbody>
</table>

## Environment Variables Used by the Agent Probe-Loader Shell Script

<table>
<thead>
<tr class="header">
<th><p>Name</p></th>
<th><p>Value</p></th>
<th><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>SYSDIG_AGENT_DRIVER</code></p></td>
<td><code>kmod</code>, <code>universal_ebpf</code>, or <code>legacy_ebpf</code></td>
<td><p>Optional. The syscall capture driver that is used by the agent. Agent defaults to `kmod` if this environment variable is not set.</p></td>
</tr>
<tr class="even">
<td><p><code>SYSDIG_BPF_PROBE</code></p></td>
<td><p><code>""</code> or a path to a custom-built eBPF object file.</p></td>
<td><p>Optional. Deprecated and superseded by <code>SYSDIG_AGENT_DRIVER</code>.  The old environment variable that is used to force the agent to load the current eBPF driver.  </p><p><b><i>Note:</i></b>The agent will exit with an error if <code>SYSDIG_AGENT_DRIVER</code> and <code>SYSDIG_BPF_PROBE</code> are set to conflicting values.</p></td>
</tr>
</tbody>
</table>


Here is a sample Docker command using environment variables in an on-prem environment with a self-signed certificate:

```bash
docker run \
  --name sysdig-agent \
  --privileged \
  --net host \
  --pid host \
  -e ACCESS_KEY=<ACCESS_KEY> \
  -e COLLECTOR=<ONPREM_COLLECTOR_HOST> \
  -e COLLECTOR_PORT=6443 \
  -e CHECK_CERTIFICATE=false \
  -e TAGS=my_tag:some_value \
  -e ADDITIONAL_CONF="log:\n file_priority: debug\n console_priority: error" \
  -v /var/run/docker.sock:/host/var/run/docker.sock \
  -v /dev:/host/dev \
  -v /proc:/host/proc:ro \
  -v /boot:/host/boot:ro \
  -v /lib/modules:/host/lib/modules:ro \
  -v /usr:/host/usr:ro \
  --shm-size=350m \
  quay.io/sysdig/agent
```
