---
linkTitle: "Optimize Azure Role Entitlements"
title: "Optimize Azure Role Entitlements"
weight: "10"
aliases:
  - /en/ciem-azure-roles
description: "Use the detail drawers on the Roles page to analyze and remediate identity risks associated with individual roles and their permissions in your Microsoft Azure environment."
---

## Manage Role Entitlements with Detail Drawers

To reduce the entitlements for a particular role, click on the role name to open the detail drawer and subtabs.

The Roles page organizes everything around the Azure role.

- **Summary**: Displays the critical permissions issues detected for this user, sorted by Permission Criticality and Unused Permission Criticality.
- **Remediation Strategies**: Summarizes all the potential strategies to reduce the permissions for this user.
- **Connected IAM Resources:** Displays a summary of this role’s total granted permissions, group associations, activity, and service principals. Displays the policies to which this role is connected, sorted by unused permissions.

If Sysdig has been profiling a user for less than 90 days, you will see the following message:

We recommend a 90 day period to pass before applying remediation optimizations to establish a good baseline for used permissions.

## Understand Role Permissions

Hover over the **% Unused Permissions** column to see the permissions granted to a role:

- **Total Permissions**: The total number of permissions granted to a role
- **Unused Permissions**: The total number of unused permissions from all the connecteded entities.


## Remediation Strategies

  - Detach Users from this Role.

    All the Users that have not used any permissions from this connected role can be detached

  - Detach Service Accounts from this Role

    All the Service Accounts that have not used any permissions from this connected role can be detached

  - Detach Groups from this Role

    All the Groups that have not used any permissions from this connected role can be detached
