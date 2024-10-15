---
linkTitle: "Netsec Policy Generation"
title: "Netsec Policy Generation"
weight: "10"
aliases:
  - /en/netsec-policy-generation.html
  - /en/docs/sysdig-secure/network/policy-generation/
---

Use the following steps to generate Kubernetes Network Policies in the Sysdig Network Security Policy Tool.

## 1. Set the Scope

Define the Kubernetes entity and timeframe for which you want
to aggregate communications.

Communications are aggregated using Kubernetes metadata to avoid having additional entries that are not
relevant for the policy creation. For example, if pod A under deployment
A communicates several times with pod B under deployment B, only one
entry appears in the interface. Or, if pod A1 and pod A2, both under
deployment A, both communicate with pod B, deployment A will represent
all its pods.

1.  In the Sysdig Secure UI, select **Inventory** > **Network** from the left panel.

2.  Choose a **Cluster** and **Namespace** from the drop-down.

3.  Select the type of Kubernetes entity for which you want to create a policy:
    -   **Service**: A Service in Kubernetes is an abstraction that defines a logical set of pods and a policy by which to access them, often defined by a selector. It ensures that applications running in different Pods can communicate with each other consistently and reliably.
    -   **Deployment**: A Deployment provides declarative updates to applications in Kubernetes. It manages the creation, scaling, and updating of a set of identical Pods, ensuring that the desired number of pods are running and available at all times.
    -   **Daemonset**: A DaemonSet ensures that all (or some) nodes run a copy of a specific pod. When nodes are added to the cluster, Pods are added to them as well. When nodes are removed from the cluster, those pods are also garbage collected. DaemonSets are typically used for background tasks like logging and monitoring.
    -   **Stateful Set**: A StatefulSet is used to manage stateful applications, providing guarantees about the ordering and uniqueness of Pods. It ensures that the pods are created and deleted in a specific order and that each Pod has a stable, unique identifier, which is useful for applications that require persistent storage.
    -   **CronJob**: A CronJob in Kubernetes manages time-based jobs, similar to cron jobs in Unix/Linux systems. It schedules and runs jobs at specified intervals, allowing tasks to be executed periodically and at fixed times, such as batch processing or automated maintenance. Choose **CronJob** to see communication aggregated to the CronJob (scheduler) level, rather than the Job, which may
        generate an excess number of entries.
    -   **Job**: A Job in Kubernetes is a controller that ensures a specified number of pods successfully complete a task. Unlike Deployments or StatefulSets, Jobs are used for finite tasks and run until completion, making them suitable for tasks like data processing or batch jobs. Choose **Job** to see entries where a Job has no CronJob parent.

4.  Select the timespan - how far back in time to aggregate the
    observed communications for the entity. The interface displays
    the Ingress and Egress tables for the Kubernetes entity and
    timeframe.

## 2. Manage Ingress and Egress

The ingress/egress tables detail the observed communications for the
selected entity (pod owner) and time period.

**Granular and global assignments:** You can then cherry-pick rows to
include or exclude from the policy granularly, or establish general rules
using the drop-down global rule options.

**Understanding unresolved IPs:** For some communications, it may not be
possible to resolve one of the endpoints to Kubernetes metadata and
classify as `Service` or `Deployment`. For example, if a
microservice is communicating with an external web server, that external
IP address is not associated with any Kubernetes metadata in your cluster. The
UI will still display these entities as "unresolved IPs." Unresolved IPs
are excluded by default from the Kubernetes network policy, but can be
added manually via the ingress/egress interface.

![](/image/netsec_ingress.png)

Choose **Ingress** or **Egress** to review and edit the detected
communications:

1.  Select the scope as described above.

2.  **For in-cluster entities:** Edit the permitted communications as
    desired, by either:

    -   Selecting/deselecting rows of allowed communication, or

    -   Choosing General Ingress/Egress Rules:
        `Block All, Allow All Inside Namespace, or Allow All.`

3.  **For unresolved IPs** (if applicable): If the tool detects many
    unresolved IPs, you can:

    -   **Search results** by any text to locate particular listings

    -   **Filter results** by

        -   *Internal*: found within the cluster

        -   *External*: found outside the cluster

        -   *Aliased*: displays any given alias

        -   *Unknown*: unable to tell if internal or external.

    -   Fine-tune the handling of unknown IPs (admins only) .
        
        You can assign an alias, set the IP to "allowed" status, or add a CIDR configuration so the IP so the IP is correctly categorized and labelled.

4.  Repeat on the other table, then proceed to check the topology and/or
    generate the policy.

## 3. Use Topology Visualization

Use the Topology view to visually validate if this is the policy you
want, or if something should be changed. The topology view is a
high-level Kubernetes metadata view: pod owners, listening ports,
services, and labels.

Communications that will not be allowed if you decide to apply this
policy are color-coded red.

![](/image/netsec_topo.png)

**Pop-up detail panes:** Hover over elements in the topology to see all
the relevant details for both entities and communications.

### Review Applied Policies 

Once policies have been generated, you can view the network policies applied to a cluster for a particular workload or workloads. 

![](/image/netsec-view-policies.png)

You can: 

* Review the relevant policies applied to the pod-to-pod communication for the current view

* Click `View Policy` to see the raw `yaml` output of the network policy applied to that workload.

![](/image/netsec-view-policies-detail.png)

### Topology Legend

When glancing at the topology, the color codes indicate:

* **Lines:** 

  Black = resolved connection  

  Red = connection not resolved; communication not included in the generated policy. Go to Ingress/Egress panels and select the relevant rows to allow the communication.

* **Entities:** 

  Blue =  the selected workload 

  Black = other services and deployements the selected workload communicates with

## 4. Review and Download Generated Policy

If you are satisfied with the rules and communication lines, simply
click the **Generated Policy** tab to get an instantaneously generated
file.

Review the resulting YAML file and download it to your browser.

![](/image/netsec_policy.png)

## Sample Use Cases

**In all cases, you begin by leaving the application running for at least 12 hours, to allow the agent to collect information.**

#### Case 1: Only Allow Specified Ingress/Egress Communications

As a developer, you want to create a Kubernetes network policy that only
allows your service/deployment to establish ingress and egress network
communications that you explicitly allow.

-   **Select the cluster namespace and deployment for your
    application.**
    You should see pre-computed ingress and egress tables. You know the
    application does not communicate with any external IP for ingress or
    egress, so you should not see any unresolved IPs. The topology map shows
    the same information.

-   **Change a rule:** You decide one service your application is
    communicating with is obsolete. You uncheck that row in the egress
    table.

-   **Check the topology map.** You will see the communication still
    exists, but is now drawn in red, meaning that it is forbidden using
    the current Kubernetes network policy (KNP).

-   **Check the generated policy code.** Verify that it follows your
    plan:
    -   No ingress/egress raw IP
    -   No entry for the service you explicitly excluded

-   **Download the generated policy** and upload it to your Kubernetes
    environment.

-   **Verify** that your application can only communicate with the
    services that were marked in black in the topology and checked in
    the tables. Then generate and download the policy to apply it.

#### Case 2: Allow Access to Proxy Static IPs

As a developer, you know your application uses proxies with a static IP
and you want to configure a policy that allows your application to
access them.

-   **See the proxy IPs** in the egress section of the interface

-   **Use the `Allow Egress to IP ` mask to create a manual rule** to
    allow those IPs in particular

-   **De-select all the other entries** in the ingress and egress tables

-   **Looking at the topology map, verify** that only the communications
    to these external IPs are marked in black, the other communications
    with the other services/deployments are marked in red

-   **Download the generated Kubernetes network policy** and apply it.

#### Case 3: Allow Communication Only Inside the Namespace

You know that your application should only communicate inside the
namespace, both for ingress and for egress.

-   **Allow ingress inside the namespace** using the general rules

-   **Allow egress inside the namespace using** the general rules

-   **Generate the policy and confirm:** everything inside the namespace
    is allowed, without nominating a particular service/deployment, then
    apply it.

#### Case 4: Allow Access to a Specified Namespace, Egress Only

Your application deployment A only communicates with applications in
deployment B, which lives in a different namespace. You only need that
egress traffic; there is no ingress traffic required for that
communication.

-   **Verify that the ingress table is empty,** both for Kubernetes
    entities and for raw IP addresses.

-   **Verify that the only communication listed on the Egress table is
    communication with deployment B**.

-   **Download the autogenerated policy, apply it, and verify:**

    -   Your application cannot communicate with other entities inside
        A's namespace.

    -   The application can contact the cluster DNS server to resolve
        other entities.

#### Case 5: Allow Access When a Deployment Has Been Relabeled

As a developer, you want to create a policy that only allows your
service/deployment to establish ingress and egress network
communications that you explicitly allow, and you need to make a change.

- After leaving the application running for a few hours, you realize
  you **didn't tag all the namespaces involved in this policy**

  A message at the top of the view will state "you need to assign
  labels to this namespace".

- **Confirm the situation in the different views:**

  -   The generated policy should not have an entry for that
      communication

  -   The Topology map should show the connection with a red line

- **Attach a label to the namespace** that was missing it. After some
  minutes, a row shows the updated information.

- **Whitelist the connection appropriately.**

- **Generate and download the policy** and apply it.
