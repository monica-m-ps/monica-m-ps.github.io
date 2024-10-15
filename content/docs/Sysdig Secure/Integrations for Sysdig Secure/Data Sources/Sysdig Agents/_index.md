---
linkTitle: "Sysdig Agents"
title: "Sysdig Agents"
weight: "30"
aliases:
  - /en/sysdig-agents.html
  - /en/sysdig-agents/
description: "This page shows all of the Sysdig Agents that have reported into the Sysdig backend." 
---

Here you can quickly determine:

- Which agents are up-to-date, out-of-date, or approaching being out-of-date
- Which managed clusters have been detected in your cloud environment, but have not yet been instrumented with the Sysdig agent
- What environment the agents are running in, containerized or non-containerized

## Review Environment 

Select **Integrations > Data Sources | Sysdig Agents**. 

![](/image/ds_agents3.png)

The resulting page shows all detected nodes in your environment and the status of the agents installed on them, or not. The view shows nodes detected from previously installed agents on hosts and from connected cloud accounts. 

Please note that for agents deployed in a Managed Cluster (EKS/GKE/AKS), the `Cluster Name` displayed will be the value retrieved from the cloud as opposed to the value configured in the agent configmap. 

You can: 

* **See at a Glance:** Quickly identify where agents are installed: by node, cluster name, or cloud account ID.
* **Know the Status:** Check agent connection status and age.
* **Search or Filter:** Narrow the view by searching or filtering on node name, cluster name, Account ID, agent version, or agent Status. 
* **Agent Count:** View your total connected agent count over time.
* **Understand Deployment Type:** Check the agent breakdown per deployment type.
* **Install or Troubleshoot:** Link to quick steps for adding an agent or troubleshooting disconnected nodes.

### Understand Agent Status

| Status             | Description                                                  | Notes                                                        |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Never Connected**    | Cloud Accounts only. Detects nodes in a managed cluster in a cloud account connected to Sysdig, where an agent has not been deployed | Hover over the status to link to the [Helm-based agent install](https://charts.sysdig.com/charts/sysdig-deploy/) instructions. |
| **Out of date**        | Deprecated agent version. Agent support is provided for the last three minor version releases. | Hover over the status for information on [upgrading the agent](/en/upgrade-agents). |
| **Almost out of date** | On the next agent release, this agent will be deprecated. Agent support is provided for the last three minor version releases. | Hover over the status for information on [upgrading the agent](/en/upgrade-agents). |
| **Disconnected**       | A Sysdig agent on a registered Kubernetes node lost connection to Sysdig. | Hover over the status for information on how to [troubleshoot an agent installation](/en/agent-troubleshooting) |
| **Up to date**      | Your agent version is up to date.           |                                         |
| **Connected**          | The agent is connected. | This status includes agents that are **Up to date**, **Almost out of date** and **Out of date**. |


### Understand Deployment Type

| Deployment Type     | Description                                                                                    |
| ------------------- | ---------------------------------------------------------------------------------------------- |
| host                | The host agent did not detect containerized environment: virtual machine, host, bare metal server |
| host, containerised | The host agent detected the Kubernetes environment by identifying the presence of `KUBERNETES_SERVICE_HOST` environment variable.          |
| fargate             | Fargate Agent                                                                                  |

{{% callout type="note" %}}

Agent version 12.18.0 or greater is required in order to show environment type (containerised/non-containerised).

{{% /callout %}}


## Options to Add Agent 

1. Go to **Integrations > Data Sources | Sysdig Agents** and select `Add Agent`.

2. Select whether your environment is **Kubernetes**, **Linux**, **Docker** or **Fargate**, and follow the wizard instructions.