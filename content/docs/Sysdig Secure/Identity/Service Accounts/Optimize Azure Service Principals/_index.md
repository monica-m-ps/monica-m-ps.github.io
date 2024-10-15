---
linkTitle: "Optimize Azure Service Principal Entitlements"
title: "Optimize Azure Service Principal Entitlements"
weight: "10"
aliases:
  - /en/ciem-opt-azure-user
description: "You can analyze and address identity risks associated with individual Azure Service Principals and their permissions by using the detailed drawers. Simply click on individual rows on the **Service Principals** page to open the detailed drawer for further analysis."
---

## Manage Service Principal Entitlements with Detail Drawers

To reduce the entitlements for a particular service principal, click on the service principal to open the detail drawer and sub-tabs.

The Service Principals page organizes everything around the individual service principal.

- **Summary**: Displays the critical permissions issues detected for this service principal, sorted by Permission Criticality and Unused Permission Criticality.
- **Remediation Strategies**: Summarizes all the potential strategies to reduce the permissions for this service principal.
- **Connected IAM Resources:** Displays a summary of total granted permissions, group associations, activity, and secret.

If Sysdig has been profiling a service principal for less than 90 days, you will see the following message:

We recommend a 90 day period to pass before applying remediation optimizations to establish a good baseline for used permissions.

## Understand Service Principal Permissions

Mouse over the % Unused Permissions column to view the permissions:

- **Total Permissions**: The total number of permissions granted to a service principal from all the roles that the service principal is connected to.
- **Unused Permissions**: The total number of unused permissions from all the roles that connected to a service principal.

## Apply Remediation Strategies

### Detach Role from the service principal

- All the roles that are totally unused by this service principal will get this recommendation
- If there are multiple detach recommendation, they are sorted based on the largest reduction in unused permissions.

### Consolidate and Reduce Permissions

Create a new custom role for this service principal with a subset of only used permissions.

This mechanism considers all the actions taken by this service principal across all its roles and consolidate them into one service principal-specific custom role. There will only be one custom role suggestion per service principal.

### Reduce Permissions with Existing Roles

Replace the existing Role with a different Role.

Sysdig recommends replacing a connected role with an existing role that includes all the necessary permissions of the current role but with fewer overall permissions, ensuring the highest access level remains the same or lower.
