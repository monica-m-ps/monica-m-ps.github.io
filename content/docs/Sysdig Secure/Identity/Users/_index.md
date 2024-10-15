---
linkTitle: "Users"
title: "Users"
weight: "12"
aliases:
  - /en/identity-and-access.html
  - /en/ia-ciem/
  - /en/ciem-users
  - /en/identity-users
no_list: true
description: "The **Users** page provides numerous ways to sort, filter, and rank the detected user information to quickly remediate identity risks associated with individual accounts and their permissions."
---

![](/image/perm_users5.png)

## Filter and Sort Accounts

Use the sortable columns to organize and filter user accounts for assessing identity risks. You can sort user accounts based on the following criteria:

#### Unused Permission Criticality

Unused Permission Criticality focuses on unused permissions, while Permission Criticality looks at all permissions. Unused Permission Criticality is designed to help you achieve Least Permissive access.

Values: **Critical, High, Medium, Low**

#### Risk

This is a calculation of risk based on all permissions. See [Understanding Risk Scoring](/en/docs/sysdig-secure/identity/#understanding-risk-scoring) for more information.

Values: **Critical, High, Medium, Low**

#### % of Unused Permissions

This shows the number of unused permissions per total permissions for the user, shown as a percentage graph.

When remediating, immediately target the users with the greatest exposure and refine them according to the suggestions.

#### Highest Access

Highest Access offers a quick way to filter by **Access Category**. It shows this identity entity’s highest level of access according to all of its permissions. For more information, see [Understand Highest Access](/en/docs/sysdig-secure/identity/#understand-highest-access).

Values:  

* **Admin:** Admin access granted
* **Write:** Write access granted
* **Read:** Read access granted
* **Empty Access:** No permissions are granted at all

#### Findings

A finding in Cloud Infrastructure Entitlement Management (CIEM) indicates poor security hygiene, either due to misconfiguration or inadequate identity security practices. The findings on User pages include:

* `No MFA`
* `Admin`

**AWS-Specific**

* `Access Key Not Rotated`
* `Multiple Access Keys Active`
* `Root User`
* `Inactive`

**GCP-Specific**

* `Editor Role Applied`
* `Owner Role Applied`

### Available Filters

* **Search:** Free text search on terms in the resource name
* **Unused Permission Criticalitys:** By severity
* **Cloud Accounts:** Account name or account number by cloud provider. For example,  `AWS`
* **Access Categories:** `Admin`, `Write`, `Read`, or `Empty Access`
* **Policy Types:** `AWS-Managed`, `Customer`, `Inline`
* **Findings:** See [Findings](#findings)

## Next Steps

To reduce the entitlements for a particular user, click on the user name to open the detail drawer and subtabs.

- [Optimize AWS User Entitlements](/en/ciem-opt-aws-user)
- [Optimize GCP User Entitlements](/en/ciem-opt-gcp-user)
- [Optimize Azure User Entitlements](/en/ciem-opt-azure-user)
