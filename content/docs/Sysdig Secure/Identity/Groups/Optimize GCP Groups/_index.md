---
linkTitle: "Optimize GCP Group Entitlements"
title: "Optimize GCP Group Entitlements"
weight: "10"
aliases:
  - /en/ciem-opt-gcp-groups
description: "You can analyze and address identity risks associated with GCP groups and their permissions by using the detailed drawers. Simply click on individual rows on the Groups page to open the detailed drawer for further analysis."
---

{{< callout type="info" >}}

 Review the permissions that you can grant to the Sysdig service account by enabling [domain-wide delegation](/en/gcp-domain-wide-delegation).

{{% /callout %}}

## Guidelines

While you can grant roles to groups at the organization, folder, or project level in Google Cloud, the resource collection workflow currently only considers groups defined at the project level. This means that only groups explicitly specified in the policy bindings document are evaluated during processing, and displayed on the **Identity** > **Service Identities** page. Policy inheritance is not currently supported.

## Manage Group Entitlements with Detail Drawers

To reduce the entitlements for a particular group, click on the account name to open the detail drawer and sub-tabs.

The Groups page organizes everything around the individual group.

- **Summary**: Displays the critical permissions issues detected for this group, sorted by Permission Criticality and Unused Permission Criticality.
- **Remediation Strategies**: Summarizes all the potential strategies to reduce the permissions for this group.
- **Connected IAM Resources:** Displays the users that are members of this group and the roles that are associated with the group. Roles are sorted by unused permissions.

![](/image/gcp-drawer-group.png)

If Sysdig has been profiling a group for less than 90 days, you will see the following message:

**Note**: Sysdig recommends a 90-days period to pass before applying remediation optimizations to establish a good baseline for used permissions.

## Understand Group Permissions

Hover over the **% Unused Permissions** column to see the permissions granted to a GCP group:

- **Total Permissions**: The total number of permissions granted to a group from all the roles the group is bound to.
- **Unused Permissions**:  The total number of unused permissions from all the roles that bound to a given group.

## Apply Remediation Strategies 

Click on the remediation recommendation to address the identity risk. 

<div style="max-width: 60%">
   <img src="/image/gcp-remedy-group.png" />
 </div>

### Detach Role from the Group

- All the roles that are totally unused by this group will get this recommendation
- If there are multiple detach recommendation, they are sorted based on the largest reduction in unused permissions.

### Consolidate Permissions

Create a new custom role for the group  with only a subset of  used permissions.

This mechanism considers all the actions taken by this group across all its roles and consolidate them into one group-specific custom role. There will only be one custom role suggestion per group account.

### Reduce Permissions with Existing Roles

Replace the existing role with a different role.

Sysdig recommends replacing a bound role with an existing role that contains all the permissions used by the current role but has fewer total permissions overall.



