---
linkTitle: "Service Identities"
title: "Service Identities"
weight: "15"
aliases:
  - /en/ciem-service-accounts
  - /en/docs/sysdig-secure/posture/identity-and-access/service-accounts/
description: "The **Service Identities** page is specific to GCP, highlighting the risks linked to your GCP service accounts. You can utilize the detail drawers on this page to assess and address identity risks associated with individual service accounts and their permissions within your GCP environment."
---
![](/image/service_identities.png)

## Filter and Sort Service Identities

Use the sortable columns to organize and filter service accounts for assessing identity risks. You can sort service accounts based on the following criteria:

#### Unused Permission Criticality

Unused Permission Criticality focuses on unused permissions, while Permission Criticality looks at all permissions. It is designed to help you achieve Least Permissive access.

**Values:** Critical, High, Medium, Low

#### Permission Criticality

[Permission criticality](/en/docs/sysdig-secure/identity/#understanding-permission-criticality-scoring) is derived from the threat research severities associated with each permission granted to this Service Principal through roles assigned to the Service Principal.

**Values:** Critical, High, Medium, Low

#### Risk

This is a calculation of risk based on all permissions. See [Understanding Risk Scoring](/en/docs/sysdig-secure/posture/identity-and-access/#understanding-risk-scoring) for more information.

**Values:** Critical, High, Medium, Low

#### % of Unused Permissions 

This shows the number of unused permissions per total permissions for the group, shown as a percentage graph. 

When remediating, immediately target the groups with the greatest exposure and refine them according to the suggestions.

#### Highest Access

**Values:**  

* **Admin:** Admin access granted
* **Write:** Write access granted
* **Read:** Read access granted
* **Empty Access:** No permissions are granted at all

See [Understand Highest Access](/en/docs/sysdig-secure/posture/identity-and-access/#understand-highest-access) for more information.

#### Findings 

A finding in Cloud Infrastructure Entitlement Management (CIEM) indicates poor security hygiene, either due to misconfiguration or inadequate identity security practices.  

The findings for GCP accounts focus on highly permissive Google IAM roles and key management.

* **Admin:** Admin access granted
* **Multiple Access Keys Active:** Rotating access keys is safer than maintaining multiple active keys. 
* **Editor Role Applied:** The GCP Editor role includes permissions to create and delete resources for most Google Cloud services.
* **User-Managed Key:** User-managed keys are less secure than Google-managed keys. 
* **Lateral Movement:** Sysdig leverages findings from the GCP Recommender Insights API to detect when a Service Account can move laterally from one project to another due to the roles/permissions it is granted.
* **Owner Role Applied:** The GCP project owner role includes all Editor permissions plus many others.

### Available Filters

* **Search:** Free text search on terms in the resource name
* **Unused Permission Criticalities:** By severity
* **Cloud Accounts:** GCP cloud account name/number 
* **Access Categories:** `Admin`, `Write`, `Read`, or `Empty Access`
* **Findings:** `Admin` , `Multiple Access Keys Active, Editor Role Applied, User Managed Key, Lateral Movement, Owner Role Applied