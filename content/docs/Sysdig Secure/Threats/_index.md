---
linkTitle: "Threats"
title: "Threats"
weight: "6"
aliases:
  - /en/event-dash
  - /en/threats
no_list: true
description: "The **Threats** page provide event trend analysis and at-a-glance summaries of top policies, rules, namespaces, accounts, or users with event activity over the past 31 days. From the Overviews, you can drill down into specific event feeds and details to take action."
---

## Prerequisites

Sysdig Secure (SaaS) with data sources connected:

* **Kubernetes:** Sysdig Agent [installed with Kubernetes orchestrator](/en/install-kubernetes-secure)
* **Cloud Accounts:** [Integrated Cloud accounts](/en/cloud-accounts-secure/) 
* **Hosts and Containers:** [Containers deployed on hosts](/en/install-hosts-secure/) without Kubernetes orchestration

{{% callout type="note" %}}
* If a particular type of data source is not connected, the corresponding overview will show no data.
* Only teams scoped to **Entire Infrastructure** will see the Dashboards.
{{% /callout %}}

## Access Threats

To access **Threats**:

1. Log into Sysdig Secure (SaaS).

2. Select **Threats** in the left side bar.

There are four types of **Overviews** to choose from:

- **All Threats**: All sources.
- **Kubernetes**: Resources in Kubernetes environments
- **Cloud**: AWS, GCP, and Azure environments
- **Host**: For environments using hosts and containers without Kubernetes orchestration.


## Navigate Threats

Review the top policies and rules with events, and drill down into the Threats feed or details to address them. 

Review the top activity associated with data such as location or users,  and drill down as needed.

The events displayed match the permissions of the team under which you logged in. 

### Filter Events

Select top-level filters to focus on particular subset of event data, as appropriate. 

All of the context filters apply to the widget on the page and any drill-down pages. 

#### By Severity

Toggle the coloured buttons on the top right of the dashboards to filter events by severity.

The severity levels are:

- High: The red **H** button.
- Medium: The orange **M** button.
- Low: The yellow **L** button.
- Info: The blue **I** button.
  
#### By Date

Use the date selector or double-click on a day to see the Event panel results filtered for just that day. 

Use the Date bar at the bottom of the page to adjust from six hours up to two weeks worth of data at a time. 

Each top trend panel reports on the behavior of events over the past 31 days. 

By default, the trend graphs are set to **1 week**. 

Page-specific filters are detailed in the panel descriptions below.

## All Threats

![](/image/events_overview.png)

The **Events Overview** panel provides: 

* Menu options to filter severity and download a PDF of the panel display.

* A summary of the  data sources and their connected status.

* **Events by Severity** trend graph: Change the date selection at the bottom of the page if desired, or hover over a day to see the event number summarized by severity for that day.

* **Top Policies** and **Top Rules** triggered: click on an entry to drill into the event details

* **Mitre Attack Report** by tactic and technique

  ![](/image/event_dash_mitre.png)

## Kubernetes

![](/image/k8_events.png)

#### Filters Available

* **Cluster**
* **Namespace**
* **Workload**

## Cloud

![](/image/cloud_dash.png)

#### Filters Available

*  **Platform**

*  **Account/Project/Subscription** (depending on AWS/GCP/Azure)

*  **Region** 
*  **Cloud Account User**

## Hosts

Designed for environments using containers without Kubernetes orchestration.

![](/image/hce.png)

### Filters Available

* **Host Names**
* **Containers**

## Use Cases

### Company Security Usage 

The top trend panels are designed to guide Security workflows. 

They present an overview of:

* Trends of Events in the environment over the past 31 days (in up to two-week increments)
* Policies and rules with most events, with up to 20 listings.
* Event data by date or date range 
* Clusters, Namespaces, Workloads, Cloud account IDs, Users, hosts, and containers with the most events detected

This data allow security managers to answer questions about their risk posture, such as: 

- Are my event levels trending down?
- What is my most event-prone environment?

## Sample Flows

### Identify Progress through Metrics

1. Choose the panel you want to view.
2. Filter on  segments of the infrastructure, such as specific clusters, accounts, users, hosts, or containers, as desired. 
3. Review the metrics graph to see trends. 
4. Click on days to identify the difference between them. 
5. Drill down to event feeds for further investigation. 