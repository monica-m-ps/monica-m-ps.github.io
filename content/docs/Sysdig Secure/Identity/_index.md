---
linkTitle: "Identity"
title: "Identity"
weight: "9"
no_list: true
aliases:
  - /en/identity-and-access.html
  - /en/ia-ciem/
  - /en/identity
  - /en/docs/sysdig-secure/posture/identity-and-access/
  - /en/docs/sysdig-secure/identity/iam-policies/#optimize-a-policy-globally
description: "As cloud services proliferate, so do user access policies. Most enterprises use overly permissive policies that create large attack surfaces and significant security risks. Sysdig **Identity** module helps you manage cloud infrastructure entitlement where you can review and mitigate Permission Criticalities in minutes."
---

## Prerequisites

### AWS

When your AWS accounts are successfully connected to Sysdig Secure with CIEM, Sysdig detects and analyzes the policies, roles, users, and groups you've configured in AWS for identity and access weak points and proposes remediation steps.

* [Connect a Cloud Account](/en/aws-secure/) for AWS
  * Installed with Terraform or CloudFormation Template
    * These  enable Threat Detection for Cloudtrail, which is required for CIEM to work with AWS

  * Either installation automatically creates a required Identity Access Management (IAM) role, which gives Sysdig read-only access to your AWS resources.
    * Terraform role name: `sfc-cloudbench`
    * CFT role name: `SysdigComplianceAgentlessRole`
* Adequate AWS permissions to read policies related to users, roles, and access.

### GCP

When your GCP accounts are successfully connected to Sysdig Secure with CIEM, the policies, roles, users, groups, and service accounts  you've configured in GCP are detected and analyzed for identity and access weak points, and Sysdig proposes remediation steps.

See [Connect a Cloud Account](/en/gcp-secure/) for GCP.

### Azure

When your Azure accounts are successfully connected to Sysdig Secure with CIEM, the users, roles, policies, groups, and service accounts  you've configured in Azure are detected and analyzed for identity and access weak points, and Sysdig proposes remediation steps.


See [Connect a Cloud Account](/en/azure-secure/) for Azure.

## Understand Identity

In Sysdig Secure, **Identity** works together with **Compliance** in the Sysdig Secure menu to highlight user-focused and resource-focused risks.

The interfaces highlight risk from different focal points:

* **IAM Policy:** (AWS only) This page highlights critical risks organized by IAM policies. The detail drawers present policy optimization changes to remove those risks. Optimization affects the entire policy.
* **Users:** This page highlights critical risks organized by individual users, focusing on unused permissions and inactive users. The detail drawers suggest when to consider removing a user due to inactivity and present policy optimization changes. Optimization affects the targeted user only.
* **Roles:**  This page highlights critical risks organized by role, focusing on unused permissions and unused roles.  The detail drawers suggest when to consider removing a role due to inactivity and present policy optimization changes. Optimization affects the targeted role only.
* **Groups:** This page highlights critical risks organized by group, focusing on unused permissions and unused groups.  The detail drawers suggest when to consider removing a group due to inactivity and present policy optimization changes. Optimization affects the targeted group only.
* **Service Accounts** (GCP only) This page highlights the risks associated with your GCP service accounts.

### Understanding Permission Criticality Scoring

Permissions primarily determine Permission Criticality in Identity and Access Management (IAM). The value for Unused Permission Criticality is  `n/a` if no used permissions are tracked. 

Each IAM action maps to a Permission Criticality Score corresponding to one of the following Permission Criticality labels: **Critical, High, Medium, Low**.

![](/image/risk0.png)

#### Permission Criticality Scores

Permission Criticality Scores for Permission Criticality are determined by the most critical permissions given by a policy. For example, a policy with at least one permission allowing a **Critical** action is given a **Critical** Permission Criticality Score.

![](/image/risk1.png)

#### Unused Permission Criticality

**Unused Permission Criticality** focuses on unused permissions, while **Permission Criticality** looks at all permissions. Unused Permission Criticality is designed to help you achieve the least permissive access. 

![](/image/risk2.png)

Note: The Unused Permission Criticality and Permission Criticality scores can differ if there are Used permissions with higher scores than Unused. 

![](/image/risk3.png)

### Understand the Suggested Policy Changes

The Sysdig CIEM may prompt you to optimize policies in different ways.

* **Optimize an AWS Policy Globally:** You can create an optimized policy to replace an existing policy. This "global" change affects all associated IAM entities (users, roles, groups).  

  Use the **Optimize IAM Policy** button on an **IAM Policy tab** or page. See [example](/en/docs/sysdig-secure/identity/iam-policies/#optimize-a-policy-globally).

* **Create Entity-Specific Optimized Policy:** You can create a new, entity-specific policy that applies only to a user, role, or group. This "local" change affects the policies the IAM entity is associated with but does not replace the original policy.

  Use the **Optimize IAM Policy** button on a User, Role, or Group **Detail Overview**. See an [example](/en/docs/sysdig-secure/identity/users/#create-an-optimized-user-policy).

* **Delete:** Sysdig may detect a policy that has not been used by any IAM entity. It will recommend removing this policy from your AWS environment.

### Understand Highest Access

Highest Access offers a quick way to filter by **Access Category**. It shows this identity entity's highest level of access according to all of its permissions. The categories are:

* **Admin**: Actions that match certain patterns related to permissions or administrative controls, such as account/organization management, are categorized as **Admin**.
* **Write**: Actions that modify data are categorized as **Write**. This includes subcategories like `Write`/`Delete,Write`/`Create`.
* **Read**: Actions that allow one to view data are categorized as  **Read**. This includes subcategories like `List, Read, Action, Tagging`.
* **Empty Access:** Either no policies are attached, or a policy is attached with zero permissions.

### Understand Remediation Strategies

The Sysdig CIEM offers the following remediation strategies:

- **Detach the role**: All the roles that are totally unused by the selected identity will get this recommendation. If there are multiple unused roles, they are sorted  by greatest reduction in unused permissions.
- **Consolidate and Reduce Permissions**: This recommendation is aimed at consolidating and reducing permissions associated with an identity. Sysdig evaluates all actions taken by the identity across their roles and consolidates them into a single, group-specific custom role. Only one custom role suggestion is provided per identity.
- **Reduce Permissions with Existing Rolese**: You will receive recommendations corresponding to the number of attached roles. If an existing role encompasses all the permissions of the current role but has fewer total permissions, the user will be advised to replace it. If the users haven't utilized any permissions, they will only receive a recommendation to detach the role. In cases where multiple replacement recommendations exist, they will be sorted by the greatest reduction in unused permissions.
