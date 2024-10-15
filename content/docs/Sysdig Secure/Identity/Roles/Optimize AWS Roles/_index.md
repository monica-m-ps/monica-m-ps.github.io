---
linkTitle: "Optimize AWS Role Entitlements"
title: "Optimize AWS Role Entitlements"
weight: "10"
aliases:
  - /en/ciem-opt-aws-roles
no_list: true
description: "Use the detail drawers on the Roles page to analyze and remediate identity risks associated with roles and their permissions in your AWS environment."
---

## Manage Role Entitlements with Detail Drawers

The Roles page organizes everything around the AWS role.

* **Summary:** Displays the critical permissions issues detected for this role, sorted by Permission Criticality and Unused Permission Criticality.
* **Remediation Strategies:** Summarizes all the potential strategies to reduce the permissions for this role.
* **Connected IAM Resources:** Displays a summary of this role's total granted permissions, group associations, activity, user ARN ID, and findings. Displays the policies to which this role is connected, sorted by unused permissions.

To reduce a role’s entitlements, click on the role name to open the detail drawer and subtabs. The remediation options for roles work the same way as for Users.

![](/image/role_detail_aws.png)

## Remediation Strategies

See the [AWS User Optimization Examples](/en/docs/sysdig-secure/identity/users/optimize-aws-users/#optimization-examples) and follow the same pattern for Roles. You can:

* Analyze the Role Permissions Details
* Optimize a policy globally
* Create a role-specific optimized policy
* Delete an unused policy
