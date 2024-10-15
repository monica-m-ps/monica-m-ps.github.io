---
linkTitle: "Roles"
title: "Roles"
weight: "13"
aliases:
  - /en/identity-and-access.html
  - /en/ia-ciem/
  - /en/ciem-roles
  - /en/docs/sysdig-secure/posture/identity-and-access/roles/
no_list: true
description: "The **Roles** page provides numerous ways to quickly sort, filter, and rank the detected role information to remediate identity risks associated with roles and their permissions."
---

![](/image/iam_roles.png)

## Filter and Sort Roles

Use the sortable columns to organize and filter roles for assessing identity risks. You can sort roles based on the following criteria:

#### Unused Permission Criticality

Unused Permission Criticality focuses on unused permissions, while Permission Criticality looks at all permissions. Unused Permission Criticality is designed to help you achieve Least Permissive access.

Values: `Critical, High, Medium, Low`

#### Risk

This is a calculation of risk based on all permissions. See [Understanding Risk Scoring](/en/docs/sysdig-secure/posture/identity-and-access/#understanding-risk-scoring) for more information.

Values: `Critical, High, Medium, Low`

#### % of Unused Permissions

This shows the number of unused permissions used with the role, per total permissions assigned to the role, shown as a percentage graph.

When remediating, immediately target the roles with the greatest exposure and refine them according to the suggestions.

#### Membership

For AWS, this reflects the number of users who can use this role.

For GCP, the membership number reflects the number of users, groups, and/or service accounts who are bound to this role.

#### Highest Access

Values:  

* **Admin:** Admin access granted
* **Write:** Write access granted
* **Read:** Read access granted
* **Empty Access:** No permissions are granted at all

For more information, see [Understand Highest Access](/en/docs/sysdig-secure/posture/identity-and-access/#understand-highest-access).

#### Findings

A finding in Cloud Infrastructure Entitlement Management (CIEM) indicates poor security hygiene, either due to misconfiguration or inadequate identity security practices. The findings on Roles pages include:

* `Admin`
* `Inactive`

### Available Filters

* **Search:** Free text search on terms in the resource name
* **Platform:** by provider, e.g. AWS
* **Unused Permission Criticalities:** By severity
* **Cloud Accounts:** Account name/number by cloud provider  (e.g. AWS)
* **Access Categories:** `Admin`, `Write`, `Read`, or `Empty Access`
* **Findings:** `Admin` , `Inactive`

## Next Steps

- [Optimize AWS Role Entitlements](/en/ciem-opt-aws-roles)
- [Optimize GCP Role Entitlements](/en/ciem-opt-gcp-roles)
- [Optimize Azure Role Entitlements](/en/ciem-azure-roles)
