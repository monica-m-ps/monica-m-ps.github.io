---
linkTitle: "Install Agent Components"
title: "Install Agent Components"
weight: "11"
no_list: true
aliases:
  - /en/install-components-secure/
  - /en/install-slim-agent.html
  - /en/docs/installation/sysdig-agent/agent-installation/quick-install-sysdig-agent-on-kubernetes/
description: "This page outlines the installation components required to use supported Sysdig Secure features. Different components are available in different environments."
---

To understand what each component does, see [Feature Overview](/en/install-secure).

For System Requirements, see [Installation Requirements](/en/install-requirements-secure).

### Kubernetes Clusters

A Kubernetes install can be accomplished through the [Quick Integrations Wizard](/en/install-kubernetes-secure/#use-the-integrations-wizard).

Kubernetes installations use a Helm chart, and the array of components are all included under the `sysdig-deploy` chart.

To obtain all the Sysdig Secure features, install the following components in Kubernetes clusters:

- Sysdig Agent
- Cluster Shield
- Vulnerability Host Scanner
- Kubernetes Security Posture Management (KSPM)
- Rapid Response

[**Install on a Kubernetes cluster**](/en/install-kubernetes-secure)

### Hosts

Host-based installations **using containers** support a subset of Sysdig Secure features.
Install the following  components on Hosts with containers:

- Sysdig Agent
- Vulnerability Host Scanner
- Posture Host Analyzer
- Rapid Response

Host-based installations **using packages** support a subset of Sysdig Secure features.
Install the following  components on Hosts with packages:

- Sysdig Agent
- Host Scanner

[**Install on a Host**](/en/install-hosts-secure)

### ECS on EC2

ECS on EC2 supports a subset of Sysdig Secure features.
Install the following  components on ECS on EC2:

Sysdig Agent

[**Install on an ECS cluster on EC2**](/en/install/ecs-ec2-secure/)

### Serverless

Serverless installations support a subset of Sysdig Secure features.
Install the following components on a serverless environment:

- Sysdig Serverless Agent

[**Install on Serverless**](/en/install-serverless-secure)

### Windows

Windows supports a subset of Sysdig Secure features.
Install the following components on Windows:

- Sysdig Windows Agent for Kubernetes
- Sysdig Windows Agent for Hosts

[**Install on a Windows host**](/en/install-windows-secure)

[**Install on Windows Kubernetes Environment**](/en/windows-k8s)
