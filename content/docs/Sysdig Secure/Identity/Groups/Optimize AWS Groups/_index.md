---
linkTitle: "Optimize AWS Group Entitlements"
title: "Optimize AWS Group Entitlements"
weight: "10"
aliases:
  - /en/ciem-opt-aws-groups
description: "Use the detail drawers on the Groups page to analyze and remediate identity risks associated with the AWS IAM User Group and policies."
---

## Manage Group Entitlements with Detail Drawers

The Groups page organizes everything around the group.

* **Summary:** Displays the critical permissions issues detected for this group, sorted by Permission Criticality and Unused Permission Criticality.
  * **Risk Overview**: Displays the critical permissions issues detected for this group, sorted by Permission Criticality and Unused Permission Criticality.
  * **Details:** Displays a summary of this group's details, including creation date, number of users, number of policies, and ARN details.
* **Remediation Strategies**: Displays remediation strategies, where applicable. See [Apply Remediation Strategies](#apply-remediation-strategies).
* **Connected IAM Resources:** Displays the users that are members of this group and the policies that are associated with the group. Policies are sorted by unused permissions.

To reduce entitlements for a particular Group, click on its name to open the detail drawer and subtabs. The remediation options for groups work similarly to users and roles.

![](/image/group_detail_aws1.png)

## Apply Remediation Strategies

See the [AWS User Optimization Examples](/en/docs/sysdig-secure/identity/users/optimize-aws-users/#optimization-examples) and follow the same basic pattern for Groups. You can:

* Analyze the group permissions details

* Create a group-specific optimized policy

* Optimize a policy globally.

  For more information, see the [example](/en/docs/sysdig-secure/identity/iam-policies/#optimize-a-policy-globally).

* Delete an unused policy

### User Permission Warning

The Users list in the Groups detail sub-tab may display a warning emoji when a user has been assigned permissions outside the group.  We recommend streamlining user permissions and using group permissions when possible.
