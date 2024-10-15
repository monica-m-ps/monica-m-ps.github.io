---
linkTitle: "Groups"
title: "Groups"
weight: "14"
aliases:
  - /en/identity-and-access.html
  - /en/ia-ciem/
  - /en/ciem-groups
no_list: true
description: "The **Groups** page provides numerous ways to sort, filter, and rank the detected User Group to quickly remediate identity risks associated with the group’s users and policies."
---

![](/image/groups.png)

Sysdig recommends creating Groups of user accounts and assigning permissions at the Group level rather than the individual User account level to facilitate administration tasks.

## Filter and Sort Groups

Use the sortable columns to organize and filter Groups for assessing identity risks. You can sort groups based on the following criteria:

#### Unused Permission Criticality

Unused Permission Criticality focuses on unused permissions, while Permission Criticality looks at all permissions. Unused Permission Criticality is designed to help you achieve Least Permissive access.

Values: `Critical, High, Medium, Low`

#### Risk

This is a calculation of risk based on all permissions. See  [Understanding Risk Scoring](/en/docs/sysdig-secure/posture/identity-and-access/#understanding-risk-scoring) for more information.

Values: `Critical, High, Medium, Low`

#### % of Unused Permissions

This shows the number of unused permissions per total permissions for the group, shown as a percentage graph.

When remediating, immediately target the groups with the greatest exposure and refine them according to the suggestions.

#### Membership

The number of users who are part of this group.

#### Highest Access

Values:  

* **Admin:** Admin access granted
* **Write:** Write access granted
* **Read:** Read access granted
* **Empty Access:** No permissions are granted at all

See [Understand Highest Access](/en/docs/sysdig-secure/posture/identity-and-access/#understand-highest-access).

#### Findings

A finding in Cloud Infrastructure Entitlement Management (CIEM) indicates poor security hygiene, either due to misconfiguration or inadequate identity security practices. The findings on Groups page include:

* `Admin`
* `Inactive`

### Available Filters

* **Search:** Free text search on terms in the resource name
* **Platform:** by provider. For example,  AWS
* **Unused Permission Criticalitys:** By severity
* **Cloud Accounts:** Account name/number by cloud provider. For example, GCP
* **Access Categories:** `Admin`, `Write`, `Read`, or `Empty Access`
* **Findings:** `Admin` , `Inactive`

## Next Steps

- [Optimize AWS Group Entitlements](/en/ciem-opt-aws-groups)
- [Optimize GCP Group Entitlements](/en/ciem-opt-gcp-groups)
- [Optimize Azure Group Entitlements](/en/ciem-opt-azure-groups)
