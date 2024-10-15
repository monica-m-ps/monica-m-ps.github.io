---
linkTitle: "KSPM"
title: "KSPM"
weight: "12"
no_list: true
aliases:
  - /en/install-reqs-kspm
  - /en/docs/installation/sysdig-secure/install-agent-components/installation-requirements/kspm/
description: The following Kubernetes platforms are supported when installing Kubernetes Security Posture Management (KSPM) in **Compliance** on Sysdig Secure.".
---
{{% callout type="note" %}}

KSPM is not yet supported in on-premises environments. For on-premises environments, consider using the [Legacy Compliance](/en/docs/sysdig-secure/posture/compliance/legacy-versions/compliance-unified/) module.

{{% /callout %}}

## Kubernetes Platforms

- Kubernetes (Vanilla)
- Amazon Elastic Kubernetes Service (EKS)
- Google Kubernetes Engine (GKE)
- Azure Kubernetes Service (AKS)
- RedHat Openshift
- IBM Kubernetes Service (IKS)

## Linux Distributions

- Ubuntu
- Debian
- RHEL
- Fedora CoreOS

## CPU Architectures

x86_64 

## Network Requirements

Make sure to allow traffic on port `12000` for communication within the cluster.

## Next Steps

[Install on a Kubernetes cluster](/en/install-kubernetes-secure)
