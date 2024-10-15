---
linkTitle: "Installation Requirements"
title: "Installation Requirements"
weight: "1"
no_list: true
aliases:
  - /en/docs/installation/sysdig-agent/agent-installation/agent-installation-requirements/
  - /en/install-requirements-secure/
  - /en/host-requirements-for-agent-installation.html
  - /en/docs/installation/sysdig-secure/install-agent-components/installation-requirements/
description: "Before installing the Sysdig agent and other components, ensure that your system meets the following requirements."
---

- A supported distribution or Kubernetes platform
- A Sysdig account and [agent access key](/en/agent-access-key/)
- Port 6443 open for outbound traffic
  
  The Sysdig Agent communicates with the collector on port 6443. If you're using a firewall, make sure to open port 6443 for outbound traffic so that the agent can communicate with the collector.
  
- Determine the [Agent Mode](/en/configure-agent-modes)
- Allow traffic on port `12000` to communicate within the cluster for Kubernetes Security Posture Management (KSPM).

## Supported across all Sysdig Components

### Kubernetes Platforms

- Kubernetes (Vanilla)
- Amazon Elastic Kubernetes Service (EKS)

  **Note**: AWS Fargate is not supported on EKS
- Google Kubernetes Engine (GKE)
- Azure Kubernetes Service (AKS)
- RedHat Openshift
- IBM Kubernetes Service (IKS)

### Linux Distributions

- Debian
- Red Hat Enterprise Linux (RHEL)
- Amazon Linux
- Amazon Linux 2
- Google Container-Optimized OS (COS)

### CPU Architectures

- X86
- ARM

We support additional Linux distributions depending on the feature required.

## For Specific Components

See individual pages for component installation requirements and supported platforms:

- [Sysdig Agent](/en/install-reqs-agent-secure/)
- [Host Scanner](/en/install-reqs-host-scan)
- [KSPM](/en/install-reqs-kspm)
- [ECS Fargate](/en/install-ecs-fargate-secure/)

## Next Steps

[**Install on a Kubernetes cluster**](/en/install-kubernetes-secure)

[**Install on a Host**](/en/install-hosts-secure/)

[**Install on an ECS cluster on EC2**](/en/install/ecs-ec2-secure/)

[**Install on an ECS Fargate cluster**](/en/install-ecs-fargate-secure/)
