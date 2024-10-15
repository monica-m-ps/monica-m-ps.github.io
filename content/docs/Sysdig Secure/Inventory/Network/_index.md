---
linkTitle: "Network"
title: "Network"
weight: "3"
aliases:
  - /en/network
  - /en/network-224583.html
  - /en/docs/sysdig-secure/network/
description: "Sysdig Network Security tracks ingress and egress communication from every pod. The Network Security Policy tool lets you generate [Kubernetes Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/) (KNPs) based on the traffic allowed or denied as defined in the Ingress and Egress tabs. You can also view which policies are being applied in real time."
---

## Prerequisites
**Sysdig agent:** `10.7.0+`

**Supported CNI Plugins:**

-   Calico
-   Weave
-   Cilium
-   OVS

**Coverage Limits**

- Communications to/from k8s nodes are not recorded.
- Workloads with no recorded communications are not present in the workloads list.

## Understand the Network Security Policy Tool

By default, all pods in a Kubernetes cluster can communicate with each other without any restrictions. [Kubernetes Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/) help you isolate the microservice applications from each other, to limit the blast radius and improve the overall security posture.

With the Network Security Policy tool, you can generate and fine-tune Kubernetes network policies within Sysdig Secure. Use it to generate a "least-privilege" policy to protect your workloads, or view existing network policies that have been applied to your workloads. Sysdig leverages native Kubernetes features and doesn't require any additional networking requirements other than the CNIs already supported.

### Benefits

Sysdig Network Security tool provides:

-   Out-of-the-box visibility into network traffic between applications
    and services, with a visual topology map to help identify
    communications.
-   A baseline network policy that you can directly refine and modify to
    match your desired declarative state.
-   Automated KNP generation based on the network communication
    baseline + user-defined adjustments.
-   Least-privilege: KNPs follow an allow-only model - it forbids any communication
    that is not explicitly allowed.
-   Enforcement delegated to the Kubernetes control plane, avoiding
    additional instrumentation or directly tampering with the host's
    network configuration
-   Map workloads to network policies applied to your cluster, helping operators and developers understand why a pods communication may or may not be blocked.
-   The ability to view the network policies applied to a cluster for a particular workload or workloads, with drill-down details to the raw yaml.

## Access the Tool 

To access **Network**:

1. Log in to Sysdig Secure.

2.  Select **Inventory** > **Network**. 

  Sysdig will prompt you to **Select a cluster + namespace + workload** then displays the Network Security Policies page.

You can now generate policies, review and tune them, and finesse configurations or troubleshoot. 