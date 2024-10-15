---
linkTitle: "Add New Features"
title: "Add New Features"
weight: "10"
no_list: true
aliases:
  - /en/azure
  - /en/azure/add-new-features
description: "After you connect your Azure environment to Sysdig, you can add additional features such as Cloud Detection and Response (CDR), Cloud Infrastructure Entitlement Management (CIEM) or Vulnerability Management Host Scanning."
---

{{< callout type="info" >}}

Sysdig released a new onboarding experience for Azure in August 2024. If you onboarded your Azure tenant and/or subscription before the 6th of August, 2024, and would like to add more features, contact your Sysdig representative.
{{% /callout %}}

## Configure Cloud Detection and Response (CDR) and Identity and Access Management (CIEM)

Both CDR and CIEM rely on processing usage logs from your cloud account. In order to enable these features, you must configure Log Ingestion. 

Setting up Log Ingestion relies on the following Azure features:
- Diagnostic Settings
- Event Hub
- Service Principals

For additional information on resources created, see [Resources created](/en/docs/sysdig-secure/connect-cloud-accounts/azure/permissions-and-resources/#resources-created-1).

### Prerequisites

- You must have an Azure Subscription or Tenant already connected to Sysdig. See [Connecting Azure Acccounts](/en/docs/sysdig-secure/connect-cloud-accounts/azure/).
- Access to a User with the [permissions required to install](/en/docs/sysdig-secure/connect-cloud-accounts/azure/permissions-and-resources/#permissions-required-to-install-1).

### Set Up Log Ingestion
1. Log in to Sysdig Secure, select **Integrations** > **Cloud Accounts** > **Azure**.
2. Select an account that is part of the tenant you would like to add features to or the individual account you onboarded.
   On the right panel, you will see a list of features.
3. Click **Setup** beside a desired feature to open the wizard.
4. Ensure you have the necessary permissions configured as described in the initial setup. 
5. Verify the details of your tenant and the subscription where the features will be added.
6. Select the region where the Event Hub will be deployed to forward logs to Sysdig (to set up log ingestion).
   We recommend that you use your primary region.
7. Generate and apply the Terraform code:
   1. Create a `log_ingestion.tf` file in the folder that contains your `main.tf`. 
   2. Copy the snippet provided into the `log_ingestion.tf` file. 
   3. Run the command:
      `terraform init && terraform apply`.

### Customize Log Ingestion
When you configure a Log Ingestion component, both Identity and Access Management and Cloud Detection and Response are enabled by default.

To disable either of these features, follow the instructions below.

1. Retrieve the `log_ingestion.tf` Terraform snippet as described above.
2. Before running `terraform init && terraform apply`, comment out the relevant stanza from the snippet.

   We recommend commenting out the stanza instead of removing it entirely, in case you want to enable the feature in the future.

   1. To disable Identity and Access Management, comment out:
       ```
       resource "sysdig_secure_cloud_auth_account_feature" "identity_entitlement" {
           account_id = module.onboarding.sysdig_secure_account_id
           type       = "FEATURE_SECURE_IDENTITY_ENTITLEMENT"
           enabled    = true
           components = [module.event-hub.event_hub_component_id]
           depends_on = [module.event-hub, sysdig_secure_cloud_auth_account_feature.config_posture]
       }
       ```
   2. To disable Cloud Detection and Response, comment out:
       ```
       resource "sysdig_secure_cloud_auth_account_feature" "threat_detection" {
           account_id = module.onboarding.sysdig_secure_account_id
           type       = "FEATURE_SECURE_THREAT_DETECTION"
           enabled    = true
           components = [module.event-hub.event_hub_component_id]
           depends_on = [module.event-hub]
       }
       ```
3. Run `terraform init && terraform apply`

#### Customize Log Ingestion after Installation

Disabling or re-enabling features after applying the Terraform is possible, simply comment or uncomment the feature stanza as described above, and re-run `terraform apply`.

### Advanced Customization

Advanced customization is available via variables in the Terraform module. Modifying these values can cause installation and/or feature operation to fail, so contact your Sysdig representative before modifying these values.

#### Customize log sources

Cloud Detection and Response (CDR) coverage of Azure includes multiple sources. By default, [Activity Logs](https://learn.microsoft.com/en-us/azure/azure-monitor/data-sources#azure-resources) are always enabled, while [Entra ID Logs](https://learn.microsoft.com/en-us/entra/identity/monitoring-health/concept-audit-logs) are enabled only in Tenant setups.

You can customize this through the Sysdig Secure UI, or in the Terraform snippet.

To customize this through the UI:

1. Log in to Sysdig Secure.

2. Select **Integrations** > **Azure**.

3. Select **+ Add Azure Account**.

    The installation wizard opens.

4. On step 4, **Select Sources (optional)**, choose which sources you would like to use under **Choose log categories**.

To customize this directly in the Terraform, use the [`enabled_platform_logs`](https://github.com/sysdiglabs/terraform-azurerm-secure/blob/main/modules/integrations/event-hub/README.md#input_enabled_platform_logs) and [`enabled_entra_logs`](https://github.com/sysdiglabs/terraform-azurerm-secure/blob/main/modules/integrations/event-hub/README.md#input_enabled_entra_logs) parameters.

{{< callout type="info" >}}

  Tuning the configuration will change the logs that will be sent to Sysdig. This will affect Sysdig's runtime visibility and threat detection capabilities.

  {{% /callout %}}

#### Configure Additional Resources

On top of subscription and tenant-wide resources, Sysdig provides CDR capabilities on additional resources, such as Key Vaults. To enable this, you must specify the resource you want to ingest logs for and the related Log Category that you want to send to Sysdig:

**Prerequisites**: An Azure cloud account connected to Sysdig through modular onboarding.

1. Log in to Sysdig Secure.

2. Select **Integrations** > **Azure**.

3. Select a connected Azure account from the list. Or select **Connected** > **Open the drawer to see more** on the listing.

  The detail drawer will open.

4. Under **Cloud Detection and Response**, select **Setup Cloud Detection and Response**.

  The Account Overview page appears.

5. Under **Cloud Detection and Response**, select **Go to Setup**.

6. Proceed through the steps. 

7. On step 5, **Configure Additional Resources (optional)**, you will be prompted to gather your Resource IDs and related log types you need. You will need them later to configure the `additional_resources` Terraform module. This module creates diagnostic settings on the resources you specify. This lets you send those logs to Sysdig through the dedicated Event Hub. Read more on the [Terraform module documentation](https://github.com/sysdiglabs/terraform-azurerm-secure/blob/main/modules/integrations/additional-resources/README.md#input_diagnostic_settings).

8. On step 8, **Deploy Terraform**, you will be provided with a snippet at point 1. Follow the instructions to copy it in the `log_ingestion.tf` file. At the end of the file add the `additional_resources` module with the `diagnostic_settings` variable set to map each Resource ID to the Log Categories you want to send to Sysdig as described at the previous step.

This example enables `AuditEvent` on a Key Vault named Foo:

```
module "additional-resources" {
  source                         = "sysdiglabs/secure/azurerm//modules/integrations/additional-resources"
  sysdig_authorization_id        = module.event-hub.sysdig_authorization_id
  event_hub_name                 = module.event-hub.event_hub_name
  deployment_identifier          = module.event-hub.unique_deployment_id
  diagnostic_settings = {
    "/subscriptions/aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee/resourceGroups/my-resource-group/providers/Microsoft.KeyVault/vaults/Foo" = ["AuditEvent"]
  }
}
```

Find available log categories for each resource type in the [Azure documentation](https://learn.microsoft.com/en-gb/azure/azure-monitor/reference/supported-logs/logs-index).

#### Tune Event Hub

Sysdig provides a default configuration for Event Hub that relies on a standard tier Event Hub with 4 partitions and throughput unit autoscaling enabled, starting from 1 throughput unit (TU) and capped at 20 maximum TUs.

To customize the number of partitions:

1. Log in to Sysdig Secure.

2. Select **Integrations** > **Azure**.

3. Select a connected Azure account from the list. Or select **Connected** > **Open the drawer to see more** on the listing.

  The detail drawer will open.

4. Under **Cloud Detection and Response**, select **Setup Cloud Detection and Response**.

  The Account Overview page appears.

5. Under **Cloud Detection and Response**, select **Go to Setup**.

6. Proceed through the steps. 

7. On step 6, **Customize Azure EventHub partitions (optional)**, select how many partitions you want. The available number ranges between 1 and 32. The default value is 8.

In general, for all the Event Hub parameters, you can adapt the arguments of the threat detection Terraform module (`source = sysdiglabs/secure/azurerm//modules/services/event-hub-data-source`). See [module specifications](https://github.com/sysdiglabs/terraform-azurerm-secure/tree/main/modules/services/event-hub-data-source#inputs) and [the Event Hub documentation](https://learn.microsoft.com/en-gb/azure/event-hubs/event-hubs-features).

### Check Connection Status

To check the connection status:

1. Log in to **Sysdig Secure** and select **Integrations** > **Cloud Accounts** > **Azure**.
2. Select your account. The Detail panel will open on the right.
If the connection is successful, you will see the feature as **Connected**. This may take up to 5 minutes after deploying the Terraform.

## Configure Vulnerability Management

This feature performs vulnerability host scanning using disk Snapshots and Lighthouse to provide highly accurate views of vulnerability risk, access to public exploits, and risk management. 

Vulnerability Host Scanning relies on the following Azure features:

- **Azure LightHouse**: Manages the relationship between the Sysdig Service Principal and the target subscriptions.
- **Snapshot**: Shares disks with Sysdig.

For additional information on resources created, see [Resources created](/en/docs/sysdig-secure/connect-cloud-accounts/azure/permissions-and-resources/#resources-created-2).

### Prerequisites

- You must have an Azure Subscription or Tenant already connected to Sysdig. See [Connecting Azure Acccounts](/en/docs/sysdig-secure/connect-cloud-accounts/azure/).
- Access to a User with the [permissions required to install](/en/docs/sysdig-secure/connect-cloud-accounts/azure/permissions-and-resources/#permissions-required-to-install-2).

### Volume Access Installation Steps

Use the following steps to set up volume access for Vulnerability Host Scanning for your Azure instances.

1. Log in to Sysdig Secure, select **Integrations** > **Cloud Accounts** > **Azure**.
2. Select an account that is part of the tenant you would like to add features to or the individual account you onboarded.
   On the right panel, you will see a list of features.
3. Click **Setup** beside a desired feature to open the wizard. 
4. Ensure you have the necessary permissions configured as described in the initial setup.
5. Exclude or Include Resources from Vulnerability Scanning:
   - You can exclude resource groups and virtual machines from scans using tags.
   - For more information, see [how to include/exclude resources](#excludingincluding-resources-from-vulnerability-scanning).
6. Verify the details of your tenant and the subscription where the features will be added.
7. Generate and apply the Terraform code:
   1. Create a `volume_access.tf` file in the folder that contains your `main.tf`.
   2. Copy the snippet provided into the `volume_access.tf` file.
   3. Run the command:
      `terraform init && terraform apply`.

### Excluding/Including Resources from Vulnerability Scanning

By default, all Resource Groups and Virtual Machines with root disks are included in scans. To manage exclusions and inclusions, use the following tags:

| Key                               | Value     | Description                    |
|-----------------------------------|-----------|--------------------------------|
| `sysdig:secure:scan`              | `true`    | Include in scan                |
|                                   | `false`   | Exclude from scan              |
| `sysdig:secure:data-volumes:scan` | `true`    | Include data volumes in scan   |
|                                   | `false`   | Exclude data volumes from scan |

### Usage Examples

| Tag                                       | Level                 | Effect                                                        |
|-------------------------------------------|-----------------------|---------------------------------------------------------------|
| `sysdig:secure:scan: "false"`             | Resource Group        | Excludes all resources within that group from scanning        |
| `sysdig:secure:scan: "false"`             | Virtual Machine       | Excludes the VM and all its disks from scanning               |
| `sysdig:secure:scan: "true"`              | Data Disk             | Includes the disk for scanning                                |
| `sysdig:secure:data-volumes:scan: "true"` | Resource Group        | Includes all data disks in that group for scanning            |
| `sysdig:secure:data-volumes:scan: "true"` | Virtual Machine       | Includes all its data disks for scanning                      |
| `sysdig:secure:data-volumes:scan: "true"` | Resource Group and VM | Excludes the VM's data-disks but includes others in the group |

### Redundant Tags

| Tag                                        | Description                                                              |
|--------------------------------------------|--------------------------------------------------------------------------|
| `sysdig:secure:scan: "true"`               | Sysdig scans by default, so these tags are redundant.                    |
| `sysdig:secure:data-volumes:scan: "false"` | Sysdig does not scan data volumes by default unless explicitly included. |

### Validate

You can verify your Vulnerability Management configuration by checking your connection status:

1. Log in to **Sysdig Secure** and select **Integrations** > **Cloud Accounts** > **Azure**.
2. Select your account. The Detail panel appears on the right.

You will see the feature as Connected. This may take up to 5 minutes after deploying the Terraform.
