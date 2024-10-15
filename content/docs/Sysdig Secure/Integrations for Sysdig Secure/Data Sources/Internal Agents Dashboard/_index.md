---
linkTitle: "Internal Agents Dashboard"
title: "Internal Agents Dashboard"
weight: "40"
aliases:
  - /en/internal-agents-dash
description: "This page is displayed only in Sysdig on-premises installations and shows granular Sysdig agent details such as associated host names, agent modes used, Kubernetes metadata status, CPU and memory usage and more."
---

## Prerequisites

* Sysdig Secure on-premises v 6.2.1+ 

*  The Cloudsec service must be enabled through a flag in the on-prem Installer: 

  `sysdig.secure.cloudsec.enabled = true`.

## Review Environment 

Select **Integrations > Data Sources | Internal Agents Dashboard**. 

![](/image/agents_dash.png)

You can: 

#### Filter and Sort

Filter by **cluster, node, or agent version** to find which agents in your environment match the criteria.

#### See at a Glance 

* **Which agent versions** you have installed, on

* **Which hostnames**

* **[Agent modes](/en/configure-agent-modes)** you defined when you installed

* **MAC addresses**

  ![](/image/agents_panel.png)

#### Review Kubernetes Metadata Up to Date

![](/image/k8s_data.png)

#### Review Internal Counts 

These panels display:

* Kernels
* Agent Flush Time
* Container Count
* System Calls Processed/Dropped
* Agent Sampling Ratio

![](/image/agents_counts.png)

#### CPU and Memory Usage 

![](/image/agent_cpu.png)







 

