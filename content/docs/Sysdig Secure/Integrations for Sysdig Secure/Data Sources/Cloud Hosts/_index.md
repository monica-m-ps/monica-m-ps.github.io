---
linkTitle: "Cloud Hosts"
title: "Cloud Hosts"
weight: "35"
aliases:
  - /en/cloud-hosts
description: "This section shows all the connected hosts, VPCs, and Resource Groups discovered with agentless vulnerability scanning. To connect an AWS account with this feature in Sysdig Secure, see the [AWS Agentless](/en/aws-agentless) installation."
---

**This feature is in Technical Preview status.** 

The two Cloud Hosts pages display details about each host, VPC, or resource group that was discovered by agentless vulnerability scanning.  You can:

- Confirm that the onboarding of agentless vulnerability scanning succeeded. 
- Review the list of hosts, VPCs, and resource groups that Sysdig discovered.
- Validate the scope of your agentless scan and troubleshoot issues.

## Hosts Tab

Select **Integrations > Data Sources | Cloud Hosts**. 

![](/image/cloud_hosts2.png)

This page provides a host scanning summary and the details about all discovered hosts.

### Host Scanning Summary

The at-a-glance graph at the top of the page summarizes the scanning status of all discovered hosts. The status can be: 

* **Scanned:** A Software Bill of Materials (SBOM) was successfully generated for the host.
* **Pending:** The host has been discovered and is pending SBOM extraction.
* **Not Scanned:** There was an issue performing SBOM extraction. You can hover over the status to get more information.
* **Skipped:** An administrator has chosen to skip the host from scanning.  For information on skipping hosts, see [Tagging Semantics](/en/docs/installation/sysdig-secure/connect-cloud-accounts/aws/agentless/#for-vulnerability-management-host-scanning-option). 
* **Not Supported:** Scanning this host is not supported. You can hover over the status to get more information or see the list of [supported hosts](/en/docs/sysdig-secure/vulnerabilities/#agentless-host-scanning).

**Total Hosts** is the cumulative number of hosts displayed with the current filters.

### Host Details

Host details include:

- **Platform:** The cloud provider of the host (AWS, GCP, Azure)
- **Hostname:** The public or private hostname
- **Instance ID:** The unique host identifier
- **Status:** The Last known state of the host
- **Account ID:** The AWS Account ID, GCP Project number, or Azure Subscription ID
- **Region:** The region in which the host is located
- **VPC / Group:** The AWS/GCP VPC or Azure Resource Group
- **Last Seen:** Time since the last Status change

## VPCs/Resource Groups  Tab

Select **Integrations > Data Sources | Cloud Hosts** and choose the **VPC/Resource Groups** tab. 

![](/image/cloud_hosts3.png) 

### Available Statuses

VPCs or Resource Groups can have one of two statuses:

- **Detected:** The VPC / Resource Group was successfully detected.
- **Skipped:** An administrator has chosen to skip the VPC/Resource Group from scanning.  For information on skipping, see [Tagging Semantics](/en/docs/installation/sysdig-secure/connect-cloud-accounts/aws/agentless/#for-vulnerability-management-host-scanning-option).

### Details

VPC/Resource Group details include: 

* **VPC/Group Name:** The given identifier
* **Status:** Detected or skipped
* **Account ID:**  The AWS Account ID, GCP Project number, or Azure Subscription ID
* **Region:** The region in which the VPC or Resource Group is located

## Filter Results

Both pages have the same filtering options. You can use free text search to filter the list of hosts by **Hostname** and **Instance ID** or VPCs / Resource Groups by their **Name**. You can also filter by:

* Platform
* Account ID
* Region
* Status

