---
linkTitle: "Optimize GCP Service Account Entitlements"
title: "Optimize GCP Service Account Entitlements"
weight: "10"
aliases:
  - /en/ciem-opt-gcp-user
description: "You can analyze and address identity risks associated with individual GCP service accounts and their permissionsby using the detailed drawers. Simply click on individual rows on the **Service Accounts** page to open the detailed drawer for further analysis."
---

{{% callout type="note" %}}

 Review the permissions that you can grant to the Sysdig service account by enabling [domain-wide delegation](/en/gcp-domain-wide-delegation).

{{% /callout %}}

## Guidelines

While you can grant roles to service accounts at the organization, folder, or project level in Google Cloud, the resource collection workflow currently only considers service accounts defined at the project level. This means that only service accounts explicitly specified in the policy bindings document are evaluated during processing, and displayed on the **Identity** > **Service Identities** page. Policy inheritance is not currently supported.

## Manage Service Account Entitlements with Detail Drawers

To reduce the entitlements for a particular service account, click on the service account to open the detail drawer and sub-tabs.

The Service Accounts page organizes everything around the individual service account.

- **Summary**: Displays the critical permissions issues detected for this service account, sorted by Permission Criticality and Unused Permission Criticality.
- **Remediation Strategies**: Summarizes all the potential strategies to reduce the permissions for this service account.
- **Connected IAM Resources:** Displays a summary of total granted permissions, role and group associations, and keys.

If Sysdig has been profiling a service account for less than 90 days, you will see the following message:

*We recommend a 90 day period to pass before applying remediation optimizations to establish a good baseline for used permissions*.

## Understand Service Account Permissions

Mouse over the % Unused Permissions column to view the permissions:

- **Total Permissions**: The total number of permissions granted to a service account from all the roles that the service account is bound to.
- **Unused Permissions**: The total number of unused permissions from all the roles that bound to a service account.

## Apply Remediation Strategies 

### Detach Role from the Service Account

- All the roles that are totally unused by this service account will get this recommendation
- If there are multiple detach recommendation, they are sorted based on the largest reduction in unused permissions.

### Consolidate and Reduce Permissions

Create a new custom role for this service account with a subset of only used permissions.

This mechanism considers all the actions taken by this service account across all its roles and consolidate them into one service account-specific custom role. There will only be one custom role suggestion per service account.

### Reduce Permissions with Existing Roles

Replace the existing Role with a different Role.

Sysdig suggests replacing a bound role with another existing role that includes all the permissions currently used by this role but has fewer total permissions overall.

