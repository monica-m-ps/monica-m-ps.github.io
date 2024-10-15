---
linkTitle: "Permissions and Resources"
title: "Permissions and Resources"
weight: "10"
no_list: true
aliases:
  - /en/azure_features
  - /en/docs/installation/sysdig-secure/connect-cloud-accounts/azure/features-and-resources/
  - /en/docs/installation/sysdig-secure/connect-cloud-accounts/azure/permissions-and-resources/
description: "This document outlines the permissions required for installing and operating various Sysdig features on Azure, as well as the resources that will be created in your Azure environment."
---

## Review Azure Roles and Permissions

### Security Principals
There are two [security principals](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-principals) involved in the onboarding process:
- **Installer**: The primary security principal, either a User or a Service Principal. This security principal will be used to perform the onboarding. Sysdig does not have access to this security principal.
- **Sysdig**: A Service Principal (robot user) created during onboarding with specific, less permissive roles. Sysdig will be given access to this security principal.

### Azure Role Types
Azure IAM is seperated into two control planes:
- [**Entra ID Roles**](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/custom-overview): Applied to the entire Tenant.
- [**Azure RBAC Roles**](https://learn.microsoft.com/en-us/azure/role-based-access-control/overview): Applied to the Subscription or Management Group being onboarded.

## Base Azure Integration - Cloud Security Posture Management (CSPM)
Agentless Cloud Security Posture Management (CSPM) assesses and manages the security posture of your cloud resources without requiring agents.

### Permissions Required to Install
The **Installer** must have at least the following roles assigned:

| Role Type  | Role                                                                                                                                               | Description                                                                             |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| Entra ID   | [Application Administrator](https://docs.microsoft.com/en-us/azure/active-directory/roles/permissions-reference#application-administrator)         | Required to create a Service Principal associated with a Sysdig-owned application.      |
| Entra ID   | [Privileged Role Administrator](https://docs.microsoft.com/en-us/azure/active-directory/roles/permissions-reference#privileged-role-administrator) | Required to assign the Directory Reader Entra ID role to the created Service Principal. |
| Azure RBAC | [User Access Administrator](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles/general#user-access-administrator)    | Required to attach Azure RBAC roles to the created Service Principal.                   |

### Permissions Granted to Sysdig
The **Sysdig** Service Principal will be granted the following roles:

| Role Type  | Role                                                                                                                                    | Description                                                                           |
|------------|-----------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|
| Entra ID   | [Directory Readers](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/permissions-reference#directory-readers) | Allows Sysdig to list Users and Service Principals.                                   |
| Azure RBAC | [Reader](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles/general#reader)                               | Allows Sysdig to list resources within your Subscriptions.                            |
| Azure RBAC | Custom Role containing: `Microsoft.Web/sites/config/list/action`                                                                        | Allows Sysdig to collect the `AuthSettings` object required by certain CSPM controls. |

### Resources Created
The following resources will be created in your Azure Environment:

| Resource                            | Description                                                                                                                                                                                                   |
|-------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `azuread_service_principal`         | Service principal used by Sysdig for secure posture management                                                                                                                                                |
| `azuread_directory_role_assignment` | Assigns the "Directory Reader" role to the **Sysdig** Service Principal                                                                                                                                       |
| `azurerm_role_assignment`           | Assigns the "Reader" role to the **Sysdig** Service Principal. For single subscription installs this is applied at on the Subscription, and for Tenant installs this is applied to the root Management Group. |
| `azurerm_role_definition`           | Custom role definition containing: `Microsoft.Web/sites/config/list/action`                                                                                                                                   |
| `azurerm_role_assignment`           | Assigns the custom role to the **Sysdig** Service Principal                                                                                                                                                   |

## Cloud Detection and Response (CDR)
Agentless Cloud Detection and Response (CDR) performs threat detection using Falco rules and policies on platform logs.

### Permissions Required to Install
The **Installer** must have at least the following roles assigned:

| Role Type  | Role                                                                                                                                               | Description                                                                                                                                    |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| Entra ID   | [Application Administrator](https://docs.microsoft.com/en-us/azure/active-directory/roles/permissions-reference#application-administrator)         | Required to create a Service Principal associated with a Sysdig-owned application.                                                             |
| Entra ID   | [Security Administrator](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/permissions-reference#security-administrator)  | Required to create Entra ID Diagnostic Settings.                                                                                               |
| Azure RBAC | [Owner](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles/general#owner)                                            | Required to attach Azure RBAC roles to the created Service Principal, and create Resource Groups, Event Hub resources and Diagnostic Settings. |

### Permissions Granted to Sysdig
The **Sysdig** Service Principal will be granted the following roles:

| Role Type  | Role                                                                                                                                                        | Description                                    |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------|
| Azure RBAC | [Azure Event Hubs Data Receiver](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles/analytics#azure-event-hubs-data-receiver) | Allows Sysdig to receive data from Event Hubs. |

### Resources Created
The following resources will be created in your Azure Environment:

| Resource                                        | Description                                                                                                                      |
|-------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| `azuread_service_principal`                     | Service principal for Event Hub integration                                                                                      |
| `azurerm_resource_group`                        | Resource group to contain the Event Hub and related resources                                                                    |
| `azurerm_eventhub_namespace`                    | Namespace for the Event Hub                                                                                                      |
| `azurerm_eventhub`                              | [Event Hub](https://learn.microsoft.com/en-us/azure/event-hubs/event-hubs-about) for log ingestion                               |
| `azurerm_eventhub_consumer_group`               | Consumer group within the Event Hub                                                                                              |
| `azurerm_eventhub_namespace_authorization_rule` | Authorization rule for the Event Hub namespace                                                                                   |
| `azurerm_role_assignment`                       | Assigns the "Azure Event Hubs Data Receiver" role to the Sysdig service principal for the Event Hub namespace                    |
| `azurerm_monitor_diagnostic_setting`            | [Diagnostic settings](https://learn.microsoft.com/en-us/azure/azure-monitor/essentials/diagnostic-settings) for the subscription |
| `azurerm_monitor_aad_diagnostic_setting`        | [Diagnostic settings](https://learn.microsoft.com/en-us/azure/azure-monitor/essentials/diagnostic-settings) for Entra ID         |

## Vulnerability Management Agentless Host Scanning
Vulnerability Management Agentless Host Scanning performs vulnerability scanning using disk snapshots and Azure Lighthouse for accurate risk assessment and management.

### Permissions Required to Install
The **Installer** must have at least the following roles assigned:

| Role Type  | Role                                                                                                                                               | Description                                                                        |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| Entra ID   | [Application Administrator](https://docs.microsoft.com/en-us/azure/active-directory/roles/permissions-reference#application-administrator)         | Required to create a Service Principal associated with a Sysdig-owned application. |
| Entra ID   | [Privileged Role Administrator](https://docs.microsoft.com/en-us/azure/active-directory/roles/permissions-reference#privileged-role-administrator) | Required to assign Entra ID roles to the created Service Principal.                |
| Azure RBAC | [User Access Administrator](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles/general#user-access-administrator)    | Required to create Azure Lighthouse Definition and Assignment.                     |


### Permissions Granted to Sysdig
The **Sysdig** Service Principal will be granted the following roles:

| Role Type  | Role                                                                                                                                                | Description                                                   |
|------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------|
| Azure RBAC | [VM Scanner Operator](https://learn.microsoft.com/en-us/azure/defender-for-cloud/faq-permissions#which-permissions-are-used-by-agentless-scanning-) | Allows Sysdig access to disk snapshot for security analysis.  |

### Resources Created
The following resources will be created in your Azure Environment:

| Resource                                   | Description                                                               |
|--------------------------------------------|---------------------------------------------------------------------------|
| `azurerm_lighthouse_definition`            | Defines the Azure Lighthouse relationship                                 |
| `azurerm_lighthouse_assignment`            | Assigns the Lighthouse definition to target subscriptions                 |
