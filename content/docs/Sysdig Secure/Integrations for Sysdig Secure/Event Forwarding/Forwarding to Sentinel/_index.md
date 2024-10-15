---
linkTitle: "Forwarding to Sentinel"
title: "Forwarding to Sentinel"
weight: "27"
aliases:
  - /en/forwarding-to-sentinel.html
  - /en/forward-sentinel
  - /en/docs/sysdig-secure/secure-events/event-forwarding/forwarding-to-sentinel/
description: "Microsoft Sentinel (formerly Azure Sentinel) is a security information and event management (SIEM) and security orchestration, automation, and response (SOAR) solution built on Azure services. See Microsoft's [Sentinel documentation](https://docs.microsoft.com/en-us/azure/sentinel/) for more details."
---

## Prerequisites

Event forwards originate from region-specific IPs. For the full list of outbound IPs by region, see [SaaS Regions and IP Ranges](/en/docs/administration/saas-regions-and-ip-ranges/#saas-regions-and-ip-ranges).  Update your firewall and allow inbound requests from these IP addresses
to enable Sysdig to handle event forwarding.

To successfully integrate Sentinel with Sysdig's event forwarding, you must have access to a configured [Log Analytics Workspace](https://docs.microsoft.com/en-us/azure/sentinel/quickstart-onboard). Go there to retrieve the **workspace ID** and **secret** you will need for the integration:

- Open your Log Analytics Workspace.
- Navigate to **Agents management** and select **Linux servers.**
- Copy the **workspace ID** and **primary key**.

## Configure Standard Integration

1. Log in to Sysdig Secure as Admin and go to **Profile > Settings > Event Forwarding**.

2. Click **+Add Integration** and choose **Microsoft Sentinel** from the drop-down menu.

3. Configure the required options:

     - **Integration Name:** Define an integration name.

     - **Workspace ID:** Enter the workspace Id you copied from the Log Analytics Workspace.

     - **Secret:** Enter the Primary key you copied from the Log Analytics Workspace.

     - **Data to Send:** Select from the drop-down the types of Sysdig data that should be forwarded to Sentinel. The available list depends on the Sysdig features and products you have enabled.

     - Toggle the enable switch as necessary. Remember that you will need to "Test Integration" with the button below before enabling the integration.

5. Click  **Save**.

## Configure Agent Local Forwarding

Review the [configuration steps](/en/event-forwarding/#configure-agent-local-forwarding) and use the following parameters for this integration.

| **Type** | **Attribute** | **Required?** | **Type** | **Allowed values** | **Default** | **Description**            |
| -------- | ------------- | ------------- | -------- | ------------------ | ----------- | -------------------------- |
| SENTINEL | workspaceId   | yes           | string   |                    |             | Log Analytics workspace ID |
| SENTINEL | secret        | yes           | string   |                    |             | Log analytics primary key  |