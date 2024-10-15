---
linkTitle: "Configuration and Troubleshooting"
title: "Configuration and Troubleshooting"
weight: "20"
aliases:
  - /en/configuration-and-troubleshooting.html
  - /en/docs/sysdig-secure/network/configuration/
---


## Kubernetes Network Configuration

**Administrators** can fine-tune how the agent processes the network data in the Configuration page.
To access the Configuration page:

1. Log in to Sysdig Secure and select **Inventory** > **Network**.

2. Select a cluster and namespace to display the Network page.

3. Click **Configuration** in the top right corner to display the Configuration page.

   It contains:
   -   Workload labels
   -   Unresolved IPs
   -   Cluster CIDR configurations

### Workload Labels

The Sysdig agent automatically detects labels used for the Kubernetes objects in a
cluster. Sometimes, there are many more labels than are required for
network security purposes. In such cases, you can select the two or
three most meaningful labels and use `include` or `exclude` namespace or workload labels to avoid clutter
in both the UI and your network security policies. For example you can exclude labels inherited by helm, and only include the labels that are required for each object (such as `app` and `name`).

![](/image/netsec-config-workload-labels.png)

### Unresolved IP Configuration

If the Sysdig agent cannot resolve an IP to a higher-level structure
(such as `Service`, `Deployment`, `Daemonset`) it will be displayed as
"unresolved" in the [ingress/egress](#section-idm53206679058414) tables. You can also add unresolved IPs from the ingress or egress tabs by clicking the `@` and creating a new alias or assigning it to an existing alias
![](/image/netsec_ip.png)

You can manually enter such IPs or CIDRs in the configuration panel,
label them with an alias, and optionally set them to "allowed" status.
Note that grouping IPs under a single alias helps declutter the
Topography view.

The following figure shows pod communication without an alias:
![](/image/netsec-egress-no-alias.png)

The following figure shows pod communication with IP aliases:
![](/image/netsec-egress-with-alias.png)


### Cluster CIDR Configuration

Unresolved IP addresses are listed and categorized as "internal" (inside the
cluster), "external" (outside the cluster) or "unknown," (subnet
information incomplete). For unknowns, Sysdig prompts you with an error
message to help you resolve it.

The simplest resolution is to manually specify cluster and service CIDRs
for the clusters.

![](/image/netsec_cidr.png)

## Troubleshooting

Tips to resolve common error messages:

### Error message: Namespaces without labels

**Problem:** Namespaces must be labeled for the Kubernetes Network Policies (KNPs) to define
ingress/egress rules. If non-labeled namespaces are detected in the
targeted communications, the Ui displays the "Namespaces without labels" error message.

![](/image/label_name.png)

**Resolution:** Simply assign a label to the relevant namespace and wait
a few minutes for the system's auto-detection to catch up.

### Error Message: Cluster subnet is incomplete

**Issue**
To categorize unresolved IPs as inside or outside the
cluster, the agent must know which CIDR ranges belong to the cluster. By
default, the agent tries to discover the ranges by examining the command
line arguments of the `kube-apiserver` and `kube-controller-manager`
processes.

If it cannot auto-discover the cluster subnets, the UI displays the "cluster subnet is
incomplete" error message.

![](/image/cluster_subnet.png)

**Resolution:**

- Preferred: Use the [Configuration](#N1622057213230) panel to add the
  CIDR entries.

- In rare cases, you may need to configure the agent to look for the
  CIDR ranges in other processes than the default
  `kube-apiserver, kube-controller-manager` processes. In this case,
  append the following to the agent configmap:

  ```yaml
  network_topology:
    pod_prefix_for_cidr_retrieval:
  [<PROCESS_NAME>, <PROCESS_NAME>]
  ```
