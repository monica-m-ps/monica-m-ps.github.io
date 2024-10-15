---
linkTitle: "Sysdig Agent"
title: "Sysdig Agent"
weight: "10"
no_list: true
aliases:
  - /en/install-reqs-agent-secure/
  - /en/docs/installation/sysdig-secure/install-agent-components/installation-requirements/sysdig-agent/
description: The following platforms, distributions, container runtimes, and CPU architectures are supported for the Sysdig agent on Sysdig Secure. 
---

## Container Platforms

Sysdig Secure supports the following platforms:

* Kubernetes v1.11 and above

  Within this, Sysdig supports:

  * Google Kubernetes Engine (GKE)
  * Amazon Elastic Kubernetes Service (EKS)
    
    Note: Amazon Web Services (AWS) Fargate is not supported on EKS
  * Azure Kubernetes Service (AKS)
  * IBM Cloud Kubernetes Service (IKS)
  * RedHat OpenShift Kubernetes Service (ROKS) v4 and above
  * Amazon ECS on EC2

## Linux Distributions

Sysdig supports the following distributions:

- Debian 
- Ubuntu 18.04 and above
- Ubuntu (Amazon)
- CentOS
- Alma Linux
- Rocky Linux
- Red Hat Enterprise Linux (RHEL)
- SuSE Linux Enterprise Server*
- RHEL CoreOS (RHCOS)
- Fedora
- Fedora CoreOS
- Linux Mint
- Amazon Linux
- Amazon Linux v2
- Amazon Linux v3
- Amazon Bottlerocket
- Google Container Optimized OS (COS)
- Oracle Linux (UEH)
- Oracle Linux (RHCK)
- Azure Linux (CBL-Mariner)
- EulerOS
  
\* Linux service install is not supported on SuSE Linux Enterprise Server.
  
## Container Runtimes

Sysdig supports the following Container Runtimes:

- Docker
- LXC
- CRI-O
- containerd*
- Podman
- Mesos

\* Standalone containers that use the containerd Runtime are not supported.

## CPU Architectures

- X86
- ARM*
- s390x (zLinux)**

\* Prebuilt probes and agent installation using the full *agent* container are not supported. Use `agent-slim` instead.

** Prebuilt probes, Captures and agent installation using the *agent* container are not supported. s390x supports only RHEL and OpenShift.

## Next Steps 

[Install on a Kubernetes cluster](/en/install-kubernetes-secure)

[Install on a Host](/en/install-hosts-secure/)

[Install on an ECS cluster on EC2](/en/install/ecs-ec2-secure/)

[Install on an ECS Fargate cluster](/en/install-ecs-fargate-secure/)
