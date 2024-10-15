---
linkTitle: "Optimize GCP Role Entitlements"
title: "Optimize GCP Role Entitlements"
weight: "10"
aliases:
  - /en/ciem-opt-gcp-roles
description: "Use the detail drawers on the Roles page to analyze and remediate identity risks associated with individual roles and their permissions in your GCP environment."
---

{{% callout type="note" %}}

 Review the permissions that you can grant to the Sysdig service account by enabling [domain-wide delegation](/en/gcp-domain-wide-delegation).

{{% /callout %}}

## Guidelines

While Google Cloud, like other public cloud providers, offers a large number of roles and permissions, only the roles bound to at least one Google IAM principal are displayed on the identity roles page. This means that any roles not specified in the policy bindings associated with a member at the project level are not evaluated during processing  and displayed on the **Identity** > **Service Identities** page. Policy inheritance is not currently supported.

## Manage Role Entitlements with Detail Drawers

To reduce the entitlements for a particular role, click on the role name to open the detail drawer and subtabs.

The Roles page organizes everything around the GCP role.

- **Summary**: Displays the critical permissions issues detected for this user, sorted by Permission Criticality and Unused Permission Criticality.
- **Remediation Strategies**: Summarizes all the potential strategies to reduce the permissions for this user.
- **Connected IAM Resources:** Displays a summary of this role's total granted permissions, group associations, activity, user accounts, and findings.

If Sysdig has been profiling a user for less than 90 days, you will see the following message:

*We recommend a 90 day period to pass before applying remediation optimizations to establish a good baseline for used permissions*.

## Understand Role Permissions

Hover over the **% Unused Permissions** column to see the permissions granted to a role:

- **Total Permissions**: The total number of permissions granted to a role
- **Unused Permissions**: The total number of unused permissions from all the bounded entities.


## Remediation Strategies 

  - Detach Users from this Role.

    All the Users that have not used any permissions from this bound role can be detached

  - Detach Service Accounts from this Role

    All the Service Accounts that have not used any permissions from this bound role can be detached

  - Detach Groups from this Role

    All the Groups that have not used any permissions from this bound role can be detached

