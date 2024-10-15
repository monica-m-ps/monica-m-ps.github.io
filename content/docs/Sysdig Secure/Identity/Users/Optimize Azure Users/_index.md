---
linkTitle: "Optimize Azure User Entitlements"
title: "Optimize Azure User Entitlements"
weight: "10"
aliases:
  - /en/ciem-opt-azure-user
description: "You can analyze and address identity risks associated with individual Microsoft Azure accounts and their permissions by using the detailed drawers. Simply click on individual rows on the Users page to open the detailed drawer for further analysis."
---

## Manage User Entitlements with Detail Drawers

To reduce the entitlements for a particular user, click on the account name to open the detail drawer and sub-tabs.

The Users page organizes everything around the individual user.

- **Summary**: Displays the critical permissions issues detected for this user, sorted by Permission Criticality and Unused Permission Criticality.
- **Remediation Strategies**: Summarizes all potential strategies to reduce the permissions for this user.

If Sysdig has been profiling a user for less than 90 days, you will see the following message:

*We recommend a 90 day period to pass before applying remediation optimizations to establish a good baseline for used permissions*.

## Understand User Permissions

Hover over the **% Unused Permissions** column to see the permissions granted to a user:

- **Total Permissions**: The total number of permissions granted to a user from all the roles the user is connected to.
- **Unused Permissions**:  The total number of unused permissions from all the roles that connected to a user.

## Remediation Strategies

### Detach Role from this User

- All the roles that are totally unused by this user will get this recommendation.
- If there are multiple detach recommendations, they are sorted based on the largest reduction in unused permissions.

### Consolidate Permissions

Create a new custom role for the user with only a subset of used permissions.

This mechanism considers all actions taken by this user across all its roles and consolidates them into one user-specific custom role. There will only be one custom role suggestion per user.

### Reduce Permissions with Existing Roles

Replace the existing role with a different role.

Sysdig recommends replacing a connected role with an existing role that contains all the permissions used by the current role but has fewer total permissions overall.

### Enable No MFA Findings

The **No MFA** finding is not enabled by default for Azure accounts. If you want to capture those findings do the following on your Azure account:

1. Log in to https://portal.azure.com/.

2. Select the appropriate Directory or Tenant.

3. Search for **Enterprise Application**.

4. Click **Enterprise Application** and click **View**.

5. On the **All Applications** page, search for **Sysdig**.

6. Select the application with the name *sysdig-secure-ENV_NAME*.

7. On the left navigation pane, click **Permissions**.

8. If you do not see **UserAuthenticationMethod.Read.All**, click the **Grant admin consent for <sysdig account>** button.

   Once you complete these settings, wait for the next scan to display the **No MFA** finding for users that have no MFA set.
