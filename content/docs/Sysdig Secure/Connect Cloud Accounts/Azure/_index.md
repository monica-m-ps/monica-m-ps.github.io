---
linkTitle: "Azure"
title: "Azure"
weight: "12"
no_list: true
aliases:
  - /en/azure
  - /en/azure-secure
description: "This topic describes the process of connecting your Azure environment to Sysdig. You can connect single subscriptions or entire tenants using Terraform. Azure coverage includes Cloud Security Posture Management (CSPM), Cloud Detection and Response (CDR), Identity and Access Management (CIEM) and Vulnerability Management (VM)."
---

## Cloud Security Posture Management (CSPM)
Connecting your Azure environment will set up a trust relationship between you and Sysdig, enabling Cloud Security Posture Management (CSPM) which:
- Monitors and detects misconfigurations in your cloud resources.
- Ensures your cloud environment complies with industry standards and regulations.
- Provides a comprehensive inventory of all cloud assets, helping you maintain visibility and control over your environment.

## Review Azure Roles and Permissions

### Security Principals
There are two [security principals](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-principals) involved in the onboarding process:
- **Installer**: The primary security principal, either a User or a Service Principal. This security principal will be used to perform the onboarding. Sysdig does not have access to this security principal.
- **Sysdig**: A Service Principal (robot user) created during onboarding with specific, less permissive roles. Sysdig will be given access to this security principal.

### Azure Role Types
Azure IAM is seperated into two control planes:
- [**Entra ID Roles**](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/custom-overview): Applied to the entire Tenant.
- [**Azure RBAC Roles**](https://learn.microsoft.com/en-us/azure/role-based-access-control/overview): Applied to the Subscription or Management Group being onboarded.

## Prerequisites

- Sysdig Secure SaaS with Admin permissions
- Terraform v1.3.1+ installed
- Azure CLI installed. See [How to install the Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).
- Access to a User with the permissions required to install.

### Permissions Required to Install
The **Installer** must have at least the following roles assigned:
- **Entra ID Roles**
  - [Application Administrator](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/permissions-reference#application-administrator): This role is required to create a Service Principal in Entra ID.
  - [Privileged Role Administrator](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/permissions-reference#privileged-role-administrator): This role is required to attach Entra ID roles to the created Service Principal. Specific roles are detailed below.
- **Azure RBAC Roles**
  - [User Access Administrator](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles/general#user-access-administrator): This role is required to attach Azure RBAC roles to the created Service Principal. Specific roles are detailed below.

### Permissions Granted to Sysdig
The installation creates a Service Principal that Sysdig can access. This Service Principal is granted the following roles:
- **Entra ID**:
  - [Directory Readers](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/permissions-reference#directory-readers): Allows Sysdig to list Users and Service Principals.
- **Azure RBAC**:
  - [Reader](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles/general#reader): Allows Sysdig to list resources within your Subscriptions.
  - Custom Role containing `Microsoft.Web/sites/config/list/action`: Allows Sysdig to collect the `AuthSettings` object required by certain CSPM controls.

## Prepare Your Environment

### 1. Configure Installation Permissions

Ensure the principal you log in to Azure with has the necessary roles and permissions to install. You can:
   - Use an existing principal who meets the permissions requirements.
   - Create a new principal and set up permissions.
   - Add permissions to an existing principal.

1. Log in to Azure.
2. Check Entra ID Roles:
   - Navigate to the Entra ID console and select **Roles and Administrators**.
   - Verify and add necessary roles.
3. Check Azure RBAC Roles:
   - For Single Subscriptions: Navigate to Subscriptions, select the target subscription, and verify roles.
   - For Management Groups: Navigate to Management Groups, select the target group, and verify roles.

### 2. Authenticate and Configure Terraform
   
A common way to do this is:

  1. **Ensure you are logged in to the correct Tenant.**

     Log in using the Azure CLI:
     ```
     az login --tenant "TENANT_ID_OR_DOMAIN"
     ```

     You will be presented with a web page to select your user account. Be sure to log in as the user you configured in Step 1.

  2. **Confirm you are logged in as the correct user**, by running:

     ```
     az ad signed-in-user show
     ```

     For alternative ways to authenticate Terraform, see the Terraform documentation:
     [Authenticating to Azure Active Directory](https://registry.terraform.io/providers/hashicorp/azuread/latest/docs#authenticating-to-azure-active-directory) and [Authenticating to Azure](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs#authenticating-to-azure).

### 3. Collect your Azure Tenant ID and Subscription ID

#### Tenant ID
1. Sign in to the Azure portal.
2. Browse to **Microsoft Entra ID > Properties**.
3. Scroll down to the Tenant ID section and you can find your tenant ID in the box.
4. Select the Copy to clipboard icon shown next to the Tenant ID. You can paste this value into a text document or other location.

#### Subscription ID
1. Sign in to the Azure portal.
2. Under the Azure services heading, select **Subscriptions**. If you don't see Subscriptions here, use the search box to find it.
3. Find the subscription in the list, and note the **Subscription ID** shown in the second column. If no subscriptions appear, or you don't see the right one, you may need to switch directories to show the subscriptions from a different Microsoft Entra tenant.
4. To easily copy the Subscription ID, select the subscription name to display more details. Select the Copy to clipboard icon shown next to the Subscription ID in the Essentials section. You can paste this value into a text document or other location.
## Install Azure Using the Wizard

1. Log in to Sysdig Secure.
2. Select **Integrations > Cloud Accounts** > **Azure** and click **Add Azure Account** on the top right corner.
3. Connect your Azure [**Tenant**](#tenant-multi-subscription) or [**Single Subscription**](#single-subscription).
   - This enables CSPM and lets you onboard Vulnerability Management and CDR after completing.

### Tenant Multi-Subscription

1. Enter your:
     - Tenant ID: The ID of the tenant you want to onboard. 
     - Subscription ID: The ID of the subscription where the Sysdig resources will be created.
2. Specify Management Groups:
   - For onboarding the entire Tenant: Enter Root Management Group ID.
   - For a subset: Enter Management Group IDs in a comma-separated list.
3. Generate and apply the Terraform code:
   1. Create a `main.tf` file.
   2. Copy the snippet provided into the file.
   3. Run the command:
      `terraform init && terraform apply`.

Within an hour after deployment, your accounts will appear on the Cloud Accounts page.

### Single Subscription

1. Enter your:
     - Tenant ID: The ID of the tenant which contains the subscription you want to onboard.
     - Subscription ID: The ID of the subscription you want to onboard.
3. Generate and apply the Terraform code:
   1. Create a `main.tf` file.
   2. Copy the snippet provided into the file.
   3. Run the command:
      `terraform init && terraform apply`.

Within an hour after deployment, your accounts will appear on the Cloud Accounts page.

### Validate

You can verify your CSPM configuration by checking the connection status:
* Log in to Sysdig Secure and select **Integrations** > **Cloud Accounts** > **Azure**.

Within 5 minutes, after you apply Terraform, your accounts will appear on the Sysdig Cloud Accounts page.
You can add more features after this initial connection by following instructions to [Add New Features](/en/azure/add-new-features).
