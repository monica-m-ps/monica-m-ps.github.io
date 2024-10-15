---
linkTitle: "Optimize GCP User Entitlements"
title: "Optimize GCP User Entitlements"
weight: "10"
aliases:
  - /en/ciem-opt-gcp-user
description: "You can analyze and address identity risks associated with individual GCP accounts and their permissionsby using the detailed drawers. Simply click on individual rows on the Users page to open the detailed drawer for further analysis."
---

{{% callout type="note" %}}

 Review the permissions that you can grant to the Sysdig service account by enabling [domain-wide delegation](/en/gcp-domain-wide-delegation).

{{% /callout %}}

## Guidelines

While you can grant roles to users at the organization, folder, or project level in Google Cloud, the resource collection workflow currently only considers users defined at the project level. This means that only users explicitly specified in the policy bindings document are collected during the scan, evaluated during processing, and displayed on the **Identity** > **Users page**. Policy inheritance is not currently supported.

## Manage User Entitlements with Detail Drawers

To reduce the entitlements for a particular user, click on the account name to open the detail drawer and sub-tabs.

![](/image/gcp-drawer.png)

The Users page organizes everything around the individual user.

- **Summary**: Displays the critical permissions issues detected for this user, sorted by Permission Criticality and Unused Permission Criticality.
- **Remediation Strategies**: Summarizes all the potential strategies to reduce the permissions for this user
- **Connected IAM Resources**: Shows all the IAM resources that are associated with the selected user. Select a resource, such as a role, group, or policy, to take further remediation actions, or view a summary of permission criticality.

If Sysdig has been profiling a user for less than 90 days, you will see the following message:

*We recommend a 90 day period to pass before applying remediation optimizations to establish a good baseline for used permissions*.

## Understand User Permissions

Hover over the **% Unused Permissions** column to see the permissions granted to a user:

- **Total Permissions**: The total number of permissions granted to a user from all the roles the user is bound to.
- **Unused Permissions**:  The total number of unused permissions from all the roles that bound to a user.

## Remediation Strategies 

### Detach Role from this User

- All the roles that are totally unused by this user will get this recommendation
- If there are multiple detach recommendation, they are sorted based on the largest reduction in unused permissions.

### Consolidate Permissions

Create a new custom role for the user with only a subset of  used permissions.

This mechanism considers all the actions taken by this user across all its roles and consolidate them into one user-specific custom role. There will only be one custom role suggestion per user.

### Reduce Permissions with Existing Roles

Replace the existing role with a different role.

Sysdig recommends replacing a bound role with an existing role that contains all the permissions used by the current role but has fewer total permissions overall.
