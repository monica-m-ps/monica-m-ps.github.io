---
linkTitle: "Identity Overview"
title: "Identity Overview"
weight: "10"
aliases:
  - /en/identity-and-access.html
  - /en/ia-ciem/
  - /en/ciem-overview
  - /en/identity-overview
description: "The Identity Overview page displays global user and permissions summaries based on overall posture, permissions, risk by user findings, and access distribution."
---

## Access the Overview

To access **Identity Overview**, log in to Sysdig Secure and select **Identity** > **Identity Overview** from the left navigation bar.

![](/image/overview.png)

### Learning Status

The Learning Status panel in the upper right corner indicates several possible states for each registered account:

- **Disconnected:** A cloud account is  `Disconnected` if no successful scan has ever occurred.
- **In Progress:** A cloud account is `In Progress` when the account was connected less than 90 days prior. This ensures that the user activity has been profiled for a meaningful amount of time.
- **Completed:** If not disconnected nor in progress, the account learning status is `Completed`. 

## Filter

Filter by **Zones**, **Platforms** and **Cloud Accounts**.


Use of the **Region** scope may result in more data being shown to users in the **Identity** pages than defined in the Zone.


## Review 

Use the graphic displays to get a birds-eye view of the most important identity and access findings in your environment. Each panel highlights a particular focus area and is interactively linked to the appropriate sub-page to take action. 

The highlights are grouped by: 

**Identity Posture** overall, featuring Inactive Groups, Roles, Policies, and Resources; Admin Groups and Admin Roles, and Users without MFA (multi-factor authentication). 

**Permissions** featuring top unused permissions to remediate, by Groups, Policies, Roles, and Users

**Risk** by User Findings

**Access Distribution** for `Read/Write/Admin` access for Roles, Users, Groups, and Policies

## CSV Download

Use the **Download CSV** button given in each page of the **Identity** module to retrieve the page data in a spreadsheet.

If your Chrome browser is set to disallow downloading multiple files from a site, you may only get one CSV download and then a “blocked” message in the Chrome address bar.

 You can click the message to access and change the browser setting.











