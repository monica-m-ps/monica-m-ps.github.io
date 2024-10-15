---
linkTitle: "Kubernetes Live"
title: "Kubernetes Live"
weight: "2"
aliases:
  - /en/secure-live
  - /en/docs/sysdig-secure/insights/secure-live/
  - /en/k8-live
description: "Kubernetes Live is a unified Cloud, Kubernetes, and Container security framework that provides visibility to the Sysdig Secure platform across your infrastructure in the last 24 hours."
---

Kubernetes  Live is a powerful tool that assists in the response and investigation into security events, vulnerabilities, and misconfigurations in your infrastructure under one pane of glass, with a simple way to scope the part of the infrastructure you are investigating.

Kubernetes  Live presents the last 24 hours of your infrastructure by scopes based on the hierarchy, such as cluster, namespace, and workloads. Selecting one of these scopes presents existing data from different parts of Sysdig Secure in a curated set of panels and tabs that are specific to that scope. As this feature evolves, more panels and "Live" views will be added, such as Posture Tab and Cloud Live.

{{% callout type="note" %}}

Kubernetes  Live currently contains the **Kubernetes Live** views and is in Technical Preview status for Sysdig Secure SaaS as part of the Sysdig Platform offering. 

Known Limitations: 

- **Limited data retention**: Secure Live only stores data for the last 24 hours
- **No customization**: The scopes, panels, and tabs in Secure Live cannot be customized at this time

{{% /callout %}}

## Access Kubernetes  Live

To access Kubernetes Live:

1. Log in to Sysdig Secure as an Administrator User.
2. Select **Inventory** > **Kubernetes Live**.


## Kubernetes Live

Kubernetes Live helps you respond to and protect your Kubernetes environment from a wide range of threats. It can be used for reactive investigation to security events or proactive highlighting images that have known vulnerabilities.

There are two main parts of Kubernetes Live:

* The left pane used as the scope tree for navigating your infrastructure 
* The right pane with security-related finding, events, and more.

![](/image/live-tree.png)

### Left Pane Scope Tree

The scope tree has three main functionalities

- **Infrastructure Layout**:  Provides a hierarchical structure of the infrastructure that Sysdig has secured for some period of time in the last 24 hour period. In Kubernetes, this is all *Clusters*, *Namespaces* & *Workloads*.
- **Panel View:** Depending on the hierarchy you select, the curated tabs and panels will change to be relevant for the level you've selected, such as *Namespace*.
- **Scope:** Filters the data that will be presented in the curated tabs, such as selecting your production cluster, or default namespace. Only that data will show in the right panel.

The scope tree is the live view (last 24 hours) of your infrastructure grouped by Cluster, Namespace, and Workload.

{{% callout type="note" %}}

As pods and containers are ephemeral, you may end up having 10s or 100s of pods or containers shown in the tree that no longer exist. For that reason, the scope tree does not display pods and containers directly. The tabs and panels provide the information needed to investigate pods and containers, even if they are no longer running.

{{% /callout %}}

#### **Scoping**

As you move down the tree, the scopes are always additive. 

For example, you can't scope to a namespace without a cluster. This also includes any filters that are available in any of the tabs, it will be the scope below + any new filter you add. 

| **Infrastructure** | **Runtime Scope**                                            |
| ------------------ | ------------------------------------------------------------ |
| Cluster            | `kubernetes.cluster.name`                                    |
| Namespace          | `kubernetes.cluster.name` AND `kubernetes.namespace.name`    |
| Workload           | `kubernetes.cluster.name` AND `kubernetes.namespace.name` AND `kubernetes.workload.name` |

### Right Pane Tabs and Panels

The data in the tabs and panels on the right pane are sourced from many different parts of Sysdig Secure, and scoped based on the *scope tree* selected. 

For example, selecting the `default` Namespace in your `production` Cluster would be similar to setting the scope in the Events feed or Vulnerability runtime to `kubernetes.cluster.name = production AND kubernetes.namespace.name=default`.

{{% callout type="warning" %}}

Kubernetes Live will only present the last 24 hours of data.
The scopes, panels and tabs cannot be modified or customized.

{{% /callout %}}

The tabs and panels are each curated with live details (past 24 hours) of your environment based on the scope selected. Some of the tabs take existing functionality and add the scoping, while some of the tabs and panels add new functionality or combine existing data into a single aggregate panel.

#### Overview

Overview is the most dynamic tab based on the scope you have selected in the tree. This tab shows the high-level details about your infrastructure, such as the visibility in the number of resources that have been or are being protected in the last 24 hours. 

* **New** **dashboards** for aggregated events and vulnerabilities have been added that were not previously in Sysdig. 
* **New** **Mitre ATT&CK** map that is only available in the `Entire Infrastructure` scope. Support for other scopes may be added in the future.

![](/image/live_overview.png)

#### Events

Provides the same functionality as the [Secure Events](/en/events-secure/) page. Secure Live provides two main enhancements:

- **The filter has an implicit filter** that is hidden based on the scope you have selected in the scope tree
- **The quick filters at the top change based on the scope** that you have selected in the scope tree. You will not see clusters if you select a cluster scope or lower, but there are **New** filters available the lower you go, such as *container* or *image* available in the workload scope.

![](/image/live_events.png)

#### Images

Provides the same functionality as the [Vulnerabilities Runtime]( /en/runtime) page, with an implicit filter that is hidden based on the scope you have selected in the scope tree.

![](/image/live_images.png)

#### Clusters, Namespaces, Workloads & Pods

Based on the scope selected, you will see one or more of these tabs. 

* **New** functionality has been added to aggregate the events and metrics about the objects in the scope you have selected. If available, it will drill down into the scope of the selected resource, for example when there are multiple findings, without the user needing to select the resource from the scope tree.

![](/image/live_clusters.png)

![](/image/live_workloads.png)

#### Logs

**Only available in the Workload scope.** 

Live Logs can display the tail logs for a container, which is the equivalent of running `kubectl logs -f`. This is useful for investigating container logs that may hint at what a workload is doing (provided there are logs with that level of detail).

When selecting a workload with multiple pods or containers, choose the pod and container for which you wish to view logs. Once requested, logs are streamed for 3 minutes before the session is automatically closed. You can simply re-start streaming if necessary.

{{% callout type="note" %}}

Live logs are tailed on-demand and thus not persisted. After a session is closed they are no longer accessible.

#### Network

**Only available in the Workload scope.**

{{% /callout %}}

The Network tab provides similar functionality to the existing [Network Security](/en/network) but is meant to be used as an observability feature only. The functionality to generate a network policy is removed.

![](/image/live_network.png)