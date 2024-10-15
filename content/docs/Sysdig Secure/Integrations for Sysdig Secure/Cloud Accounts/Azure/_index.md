---
linkTitle: "Azure"
title: "Azure"
weight: "30"
aliases:
  - /en/cloud-accounts-azure
description: "This topic describes how to review the account details after you connect a Microsoft Azure cloud account to Sysdig Secure. You can add additional accounts with the install wizard and validate the status of onboarded features."
---
## Access the Page

Log in to Sysdig Secure and select **Integrations > Cloud Accounts > Azure** from the navigation bar.

The Azure Cloud Accounts overview appears. 

![](/image/cloud_azure3.png)

## Add Azure Accounts

To connect an account, select **+ Add Azure Account**, and then follow the installation pop-up wizard. 

See also: [Connect Cloud Account | Azure](/en/azure) in the Installation documentation. 

## Review Azure Cloud Accounts 

The page lists:

- **Subscription:**  Azure **Subscription ID**, followed by an **Subscription Name**, if one was assigned.

- **Status:** The **Status** of each Azure Subscription you have connected. 

  - Possible values: `Connected`, `Pending`, `Partial Error`, `Error`, `Unknown`. For more on status values, see [Validate Account Connection](#validate-account-connection).

- **Last Checked:**  Time at which a feature was last validated. Validation occurs every 24 hours.

- **Tenant ID:** ID of the Organization to which the Subscription belongs, if applicable.

- **Added On:** Date the Subscription was added to Sysdig Secure.

### Validate Account Connection

There are two types integration status displayed, **Feature** level status, and **Subscription** level status. Statuses are validated approximately every 24 hours, with the first check occurring within an hour of enabling a feature.

{{% callout type="note" %}}

In the case of connection errors, if you remediate them the status will be updated on the page when the validation is run again, up to 24 hours later.

{{% /callout %}}

#### Feature Status

When you connect a cloud account to Sysdig Secure, you select the Sysdig features you would like to enable, such as Cloud Security Posture Management (CSPM), Cloud Infrastructure Entitlements Management (CIEM), Cloud Detection and Reponse (CDR) and Vulnerability Host Scanning. Features you've enabled will appear in the detail panel that opens when you select a row, where you can also see **Feature** connection status.

The possible **Feature** statuses are:

| Status Value | Description       |
|--------------|-------------------|
| Connected    | This feature was successfully onboarded and connected.  |
| Not Enabled  | This feature was not enabled during onboarding.          |
| Error        | There is an error in this feature connection.              |
| Pending      | This feature has been recently connected, and a validation will be run within an hour. |
| Unknown      | Sysdig cannot determine the current status of the feature.     |

#### Subscription Status

The **Subscription** level status is shown in the main table, as well as at the top of the details panel. This status is an aggregate of the **Feature** statuses present in that subscription.

The possible **Subscription** statuses are:

| Status Value  | Description      |
|---------------|------------------|
| Connected     | All selected features were successfully onboarded and connected. |
| Partial Error | There is an error in at least one enabled feature.   |
| Error         | There are errors in all enabled features.   |
| Pending       | The account has been recently connected, and a validation will be run within an hour. |
| Unknown       | Sysdig cannot determine the current status of the subscription.|
 