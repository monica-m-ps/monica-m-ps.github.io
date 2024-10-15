---
linkTitle: "Optimize Azure Group Entitlements"
title: "Optimize Azure Group Entitlements"
weight: "10"
aliases:
  - /en/ciem-opt-azure-groups
description: "You can analyze and address identity risks associated with Azure groups and their permissions by using the detailed drawers. Simply click on individual rows on the Groups page to open the detailed drawer for further analysis."
---

## Manage Group Entitlements with Detail Drawers

To reduce the entitlements for a particular group, click on the account name to open the detail drawer and sub-tabs.

The Groups page organizes everything around the individual group.

- **Summary**: Displays the critical permissions issues detected for this group, sorted by Permission Criticality and Unused Permission Criticality.
- **Remediation Strategies**: Summarizes all the potential strategies to reduce the permissions for this group.
- **Connected IAM Resources:** Displays the users that are members of this group and the roles that are associated with the group. Roles are sorted by unused permissions.

![](/image/azure-drawer-group.png)

If Sysdig has been profiling a group for less than 90 days, you will see the following message:

We recommend a 90 day period to pass before applying remediation optimizations to establish a good baseline for used permissions.

## Understand Group Permissions

Hover over the **% Unused Permissions** column to see the permissions granted to a Azure group:

- **Total Permissions**: The total number of permissions granted to a group from all the roles the group is connected to.
- **Unused Permissions**:  The total number of unused permissions from all the roles that connected to a given group.

## Apply Remediation Strategies

Click on the remediation recommendation to address the identity risk.

<div style="max-width: 60%">
   <img src="/image/azure-remedy-group.png" />
 </div>


### Detach Role from the Group

- All the roles that are totally unused by this group will get this recommendation
- If there are multiple detach recommendation, they are sorted based on the largest reduction in unused permissions.

### Consolidate Permissions

Create a new custom role for the group  with only a subset of  used permissions.

This mechanism considers all the actions taken by this group across all its roles and consolidate them into one group-specific custom role. There will only be one custom role suggestion per group account.

### Reduce Permissions with Existing Roles

Replace the existing role with a different role.

Sysdig recommends replacing a connected role with an existing role that includes all the necessary permissions of the current role but with fewer overall permissions, ensuring the highest access level remains the same or lower.
