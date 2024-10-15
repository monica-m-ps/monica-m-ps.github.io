---
linkTitle: "Troubleshooting"
title: "Troubleshooting"
weight: "14"
aliases: 
  - /en/troubleshooting-agents
  - /en/troubleshooting-agent-installation
  - /en/agent-troubleshooting
  - /en/agent-troubleshooting.html
  - /en/install-troubleshooting
  - /en/docs/installation/sysdig-agent/troubleshooting-agent-installation
  - /en/docs/installation/sysdig-agent/agent-troubleshooting/
---

This section describes methods for troubleshooting two types of issue:

-   Disconnecting Agents

-   No Metrics After Agent Install

## Disconnecting Agents

If agents are disconnecting, there could be problems with addresses that
need to be resolved in the agent configuration files. See also
[Understanding the Agent Config
Files](/en/configure-sysdig-agent).

### Check for Duplicate MAC addresses

The Sysdig agent will use the `eth0` MAC address to identify the
different hosts within an infrastructure. In a virtualized environment,
you should confirm each of your VM's `eth0` MAC addresses are unique.

If a unique address cannot be configured, you can supply an additional
parameter in the Sysdig agent's `dragent.yaml` configuration file:
`machine_id_prefix: prefix`

The prefix text can be any string and will be prepended to the MAC
address as reported in the Sysdig Monitor web interface's Explore
tables.

**Example:** (using `ADDITIONAL_CONF` rather than Kubernetes
`Configmap`)

Here is an example Docker run command installing the parameter via the
`ADDITIONAL_CONF` parameter

```yaml
docker run --name sysdig-agent --privileged --net host --pid host -e ACCESS_KEY=abc123-1234-abcd-4321-abc123def456 -e TAGS=tag1:value1 -e ADDITIONAL_CONF="machine_id_prefix: MyPrefix123-" -v /var/run/docker.sock:/host/var/run/docker.sock -v /dev:/host/dev -v /proc:/host/proc:ro -v /boot:/host/boot:ro -v /lib/modules:/host/lib/modules:ro -v /usr:/host/usr:ro sysdig/agent
```

The resulting `/opt/draios/etc/dragent.yaml` config file would look like
this:

```yaml
customerid:abc123-1234-abcd-4321-abc123def456
tags: tag1:value1
machine_id_prefix: MyPrefix123-
```

You will then see all of your hosts, provided that all the prefixes are
unique. The prefix will be visible whenever the MAC address is displayed
in any view.

See also: [Agent
Configuration](/en/configure-sysdig-agent).

### Check for Conflicting MAC addresses in GKE environments

In Google Container Engine (GKE) environments, MAC addresses could be
repeated across multiple hosts. This would cause some hosts running
Sysdig agents not to appear in your web interface.

To address this, add a unique machine ID prefix to each config you use
to deploy the agent to a given cluster (i.e. each
[sysdig-daemonset.yaml](https://github.com/draios/sysdig-cloud-scripts/tree/master/agent_deploy/kubernetes)
file).

**Note:** This example uses the
([v1](/en/docs/installation/)) `ADDITIONAL_CONF`, rather than ([v2](/en/docs/installation/)
`Configmap` method.

`- name: ADDITIONAL_CONF value: "machine_id_prefix: mycluster1-prefix-" `

## Can't See Metrics After Agent Install

If agents were successfully installed, you could log in to the Sysdig
Monitor UI, but no metrics are displayed in the Explore panel, first
confirm that the agent license count has not been exceeded. Then check
for any proxy, firewall, or host security policies preventing proper
agent communication to the Sysdig Monitor backend infrastructure.

### Check License Count

If network connectivity is good, the agent will connect to the backend
but will be disconnected after a few seconds if the license count has
been exceeded.

To check whether you are over-subscribed, go to
**`Settings > Subscription`.**

See [Subscription](/en/docs/administration/administration-settings/subscription/#subscription) for
details.

### Check Network Policy

#### Agent Connection Port

Check your service provider VPC security groups to verify that network
ACLs are set to allow the agent's outbound traffic over TCP ports. See
[Sysdig Collector Ports](/en/docs/administration/saas-regions-and-ip-ranges/#saas-regions-and-ip-ranges) for
the supported TCP ports for each region.

#### Outbound IP Addresses

Due to the distributed nature of the Sysdig Monitor infrastructure, the
agent must be open for outbound connections to
*[collector.sysdigcloud.com](http://collector.sysdigcloud.com)* on all
outbound IP addresses.

Check [Amazon's public IP ranges
file](https://ip-ranges.amazonaws.com/ip-ranges.json) to see all the
potential IP addresses the Sysdig agent can use to communicate with the
Sysdig backend databases.

#### AWS Metadata Endpoint

AWS metadata is used for gathering information about the instance
itself, such as instance id, public IP address, etc.

When running on an AWS instance, access to the following AWS metadata
endpoint is also needed: 169.254.169.254

### Check Local Host Policy

The agent requires access to the following local system resources in
order to gather metrics:

-   Read/Write access to `/dev/sysdig` devices.

-   Read access to all the files under `/proc` file system.

-   For container support, the Docker API endpoint
    `/var/run/docker.sock`

If any settings or firewall modifications are made, you may need to
restart the agent service. In a shell on the affected instances issue
the following command:

```yaml
sudo service dragent restart
```
