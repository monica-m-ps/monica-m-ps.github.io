---
linkTitle: "Use Node Leases"
title: "Use Node Leases"
weight: "21"
aliases:
  - /en/docs/installation/sysdig-agent/agent-installation/agent-install-kubernetes/using-node-leases/
  - /en/using-node-leases.html
  - /en/using-node-leases
  - /en/use-node-leases
  - /en/docs/installation/sysdig-agent/agent-installation/agent-install-kubernetes/using-node-leases/
---

The Sysdig agent uses [Kubernetes
Lease](https://kubernetes.io/docs/reference/kubernetes-api/cluster-resources/lease-v1/)
to control how and when connections are made to the Kubernetes API
Server. This mechanism prevents overloading the Kubernetes API server
with connection requests during agent bootup.

Kubernetes node leases are automatically created for agent version
12.0.0 and above. On versions prior to 12.0.0, you must configure node
leases as given in the KB article.

## Prerequisites

-   Sysdig Agent v11.3.0 or above

-   Kubernetes v1.14 or above

## Types of Leases

The agent creates the following leases:

### Cold Start

During boot up, the Sysdig agent connects to the Kubernetes API server
to retrieve Kubernetes metadata and build a cache. The `cold-start`
leases control the number of agents that build up this cache at any
given time. An agent will grab a lease, build its cache, and then
release the lease so that another agent can build its cache. This
mechanism prevents agents from creating a "boot storm" which can
overwhelm the API server in large clusters.

### Delegation

In Kubernetes environments, two agents are marked as `delegated` in each
cluster. The `delegated` agents are the designated agents to request
more data from the API server and produce KubeState metrics. The
`delegation` leases will not be released until the agent is terminated.

## View Leases

To view the leases, run the following:

```yaml
$ kubectl get leases -n sysdig-agent
```

You will see an output similar to the following:

```yaml
NAME           HOLDER             AGE
cold-start-0                      20m
cold-start-1                      20m
cold-start-2                      21m
cold-start-3   ip-10-20-51-167    21m
cold-start-4                      21m
cold-start-5                      21m
cold-start-6                      20m
cold-start-7                      21m
cold-start-8                      20m
cold-start-9   ip-10-20-51-166   21m
delegation-0   ip-10-20-52-53    21m
delegation-1   ip-10-20-51-98    21m
```

## Troubleshoot Leases

### Verify Configuration

When lease-based delegation is working as expected, the agent logs show
one of the following:

-   **Getting pods only for node &lt;*node*&gt;**

-   **Getting pods for all nodes**.

-   Both (occasionally on the delegated nodes)

Run the following to confirm that it is working:

```yaml
$ kubectl logs sysdig-agent-9l2gf -n sysdig-agent | grep -i "getting pods"
```

The configuration is working as expected if the output on a pod is
similar to the following:

```yaml
2021-05-05 02:48:32.877, 15732.15765, Information, cointerface[15738]: Only getting pods for node ip-10-20-51-166.ec2.internal
```

### Unable to Create Leases

The latest Sysdig ClusterRole is required for the agent to create
leases. If you do not have the latest ClusterRole or if you have not
configured the ClusterRole correctly, the logs show the following error:

```yaml
Error, lease_pool_manager[2989554]: Cannot access leases objects: leases.coordination.k8s.io is forbidden: User "system:serviceaccount:sysdig-agent:sysdig-agent" cannot list resource "leases" in API group "coordination.k8s.io" in the namespace "sysdig-agent"
```

Contact [Sysdig Support](https://sysdig.com/support/) for help.

## Optional Agent Configuration

Several configuration options exist for leases. It is recommended to not
change the default settings unless prompted by Sysdig Customer Support.

<table>
<thead>
<tr class="header">
<th><p>Configuration</p></th>
<th><p>Default</p></th>
<th><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>k8s_coldstart:
  enabled: &lt;true/false&gt;</code></pre></td>
<td><p><code>true</code> above agent versions 12.0.0</p></td>
<td><p>When true, the agent will attempt to create <code>cold-start</code> leases to control the number of agents which are allowed to build their cache at one time.</p></td>
</tr>
<tr class="even">
<td><pre><code>k8s_coldstart:
  max_parallel_cold_start: &lt;int&gt;</code></pre></td>
<td><p>10</p></td>
<td><p>The number of <code>cold-start</code> leases to be created. This is the number of agents that can connect to the API Server simultaneously during agent initialization.</p></td>
</tr>
<tr class="odd">
<td><pre><code>k8s_coldstart:
  namespace: &lt;string&gt;</code></pre></td>
<td><p><code>sysdig-agent</code></p></td>
<td><p>The namespace to be created. This shouldn't be needed in agent version 12.0.0 because the DownwardAPI in the ClusterRole will provide the appropriate namespace.</p></td>
</tr>
<tr class="even">
<td><pre><code>k8s_coldstart:
  enforce_leader_election: &lt;true/false&gt;</code></pre></td>
<td><p><code>false</code></p></td>
<td><p>When <code>true</code>, the agent will not fall back to the previous method if it cannot create leases.This can be useful if the previous method caused API Server problems.</p></td>
</tr>
<tr class="odd">
<td><pre><code>k8s_delegation_election: &lt;true/false&gt;</code></pre></td>
<td><p><code>true</code> above agent versions 12.0.0</p></td>
<td><p>When <code>true</code>, the agent will create <code>delegation</code> leases to control which set of agents generate global cluster metrics.</p></td>
</tr>
</tbody>
</table>
