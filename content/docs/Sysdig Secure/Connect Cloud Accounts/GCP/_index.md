---
linkTitle: "GCP"
title: "GCP"
weight: "11"
no_list: true
aliases:
  - /en/docs/installation/sysdig-secure-for-cloud/deploy-sysdig-secure-for-cloud-on-gcp/
  - /en/docs/sysdig-secure/sysdig-secure-for-cloud/gcp
  - /en/gcp-secureagentless/
  - /en/gcp-secureagentless-cspm-and-ciem/
  - /en/gcp-secure-agentless
  - /en/gcp-secure
  - /en/docs/installation/sysdig-secure/connect-cloud-accounts/gcp/
description: "Prepare your environment, then follow the wizard’s prompts to install agentless Cloud Security Posture Management (CSPM), Identity and Access Management (CIEM), Cloud Detection and Response (CDR), and/or Vulnerability Management host scanning on Google Cloud Platform (GCP). You can connect single projects or organizations."
---
## Prerequisites

### Installed Applications

- Sysdig Secure SaaS  with `administrator` permissions

- Terraform must be installed on the machine from which you will deploy the installation code, along with:

  - Terraform Google Platform Provider

  - Google's Cloud software development kit (SDK) must be deployed in the environment where you will deploy the installation code.

    For further guidance, see the Hashicorp and Google documentation: [Install Terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli);  [Google Platform Provider](https://registry.terraform.io/providers/hashicorp/google/latest/docs); [Install the gcloud CLI](https://cloud.google.com/sdk/docs/install).

- Have on hand:

  - For Organizations: The GCP Organization domain, Organization Member Project ID, and Region
  - For Projects: The Project ID

## Review GCP Roles and Permissions

- Review these concepts before preparing your environment and running the onboarding wizard. To assign user roles, enable APIs, and configure domain-wide delegation, you will need to log in to and access two different GCP consoles at different times:

  - [Google Cloud Console](https://console.cloud.google.com/)

  - [Google Admin Console](https://admin.google.com/)

    See  [Prepare Your Environment](#prepare-your-environment) and [Configure Domain-Wide-Delegation](#configure-domain-wide-delegation).

- Review the permissions allotted to the Sysdig service account through [domain-wide delegation](/en/gcp-domain-wide-delegation).

### User Types

If you install by hand or on your local machine, you will want likely to install as a **user**. If you are automating the installation, such as using Terraform Cloud, you will likely want to install as a **service account**.

You can:

- Use an existing user or service account that meets the permissions requirements
- Create a new user or service account and set up permissions
- Add permissions to an existing user or service account

### Permissions Required to Install

#### Single Project

The installing user/service account must have the following roles assigned on the Project that is being onboarded:

- `roles/iam.serviceAccountAdmin`
- `roles/iam.roleAdmin`
- `roles/resourcemanager.projectIamAdmin`
- `roles/iam.serviceAccountKeyAdmin`

If you are installing CDR, you must have the following **additional** roles assigned on the Project that is being onboarded:

- `roles/pubsub.admin`
- `roles/logging.configWriter`
- `roles/iam.serviceAccountUser`
- `roles/serviceusage.serviceUsageAdmin`

If you are installing Vulnerability Host Scanning, you must have the following **additional** roles assigned:

- `roles/iam.workloadIdentityPoolAdmin`

#### Organization

Note: Certain roles are required at the organization level. Other roles are required on a single project in which you will deploy shared resources. Ensure you have the correct roles assigned at the correct scope.

The installing user/service account must have the following roles assigned:

- `roles/iam.serviceAccountAdmin` (On the project where shared resources will be created)
- `roles/iam.organizationRoleAdmin` (At the Organization level)
- `roles/resourcemanager.organizationAdmin` (At the Organization level)
- `roles/iam.serviceAccountKeyAdmin` (On the project where shared resources will be created)

If you are installing CDR, you must have the following **additional** roles assigned:

- `roles/pubsub.admin` (On the project where shared resources will be created)
- `roles/logging.configWriter` (At the Organization level)
- `roles/iam.serviceAccountUser` (On the project where shared resources will be created)
- `roles/serviceusage.serviceUsageAdmin` (On the project where shared resources will be created)

If you are installing Vulnerability Host Scanning, you must have the following **additional** roles assigned:

- `roles/iam.workloadIdentityPoolAdmin` (On the project where shared resources will be created)

### Permissions Granted to Sysdig

The installation also creates a service account that Sysdig can access. This service account will be granted the following roles:

- `roles/browser`
- `roles/cloudasset.viewer`
- `roles/logging.viewer`
- `roles/recommender.viewer`
- `roles/container.clusterViewer`
- `roles/compute.viewer`
- `roles/iam.serviceAccountTokenCreator`
- `roles/iam.serviceAccountViewer`
- `roles/iam.roleViewer`
- `roles/roles/cloudfunctions.viewer` in order to retrieve all cloud functions within the project
- `roles/cloudbuild.builds.viewer` in order to retrieve the last cloud build of every existing cloud function
- `roles/orgpolicy.policyViewer` in order to retrieve organization policies for network exposure analysis

#### If Vulnerability Host Scanning is Added

- `roles/iam.workloadIdentityUser`
- `permissions/iam.servieAccounts.getAccessToken`
- `permissions/compute.zoneOperation.get`
- `permissions/compute.disk.list`
- `permissions/compute.disk.get`
- `permissions/compute.disk.useReadOnly`
- `permissions/compute.instance.list`
- `permissions/compute.instance.get`
- `permissions/compute.instance.network.list`
- `permissions/compute.instance.network.get`

## Prepare Your Environment

Preparation of your GCP environment, roles, and permissions is the key to a seamless connection between your GCP cloud accounts and Sysdig. When preparation is complete, the installation itself is a simple, wizard-guided process from the Sysdig Secure UI.

Follow each of the steps below to prepare for onboarding.

### Step 1: Provide User with Appropriate Roles

Ensure your user has the correct roles and permissions in GCP to perform the onboarding.

#### Single Project

To check or assign roles:

1. Log in to the  [Google Cloud Console](https://console.cloud.google.com/) as either a user or a service account, ensuring you have the correct project active.
2. Navigate to  **IAM & Admin > IAM**.
3. In **VIEW BY PRINCIPALS**, find your User/service account.
4. Ensure that all the roles listed in [Permissions Required to Install](#permissions-required-to-install) are present.
5. If any roles are missing, select your user/service account, and grant the roles using the **Grant Access** button. You may need to work with your administrator to be granted the correct roles.

![](/image/gcp_roles.png)

#### Organization

NOTE: Certain roles are required at the organization level. Certain roles are required on a single project in which you will deploy shared resources. Ensure you have the correct roles assigned at the correct scope.

For roles required on a single project, follow the instructions for a single project above.

For roles that are required at the organization level:

1. Log in to the  [Google Cloud Console](https://console.cloud.google.com/) as either a user or a service account.
2. Ensure the organization is selected in the project selector in the top bar. If you do not see your organization there, you may need to work with your administrator.
3. In **VIEW BY PRINCIPALS**, find your User/Super Administrator.
4. Ensure that all the roles listed in [Permissions Required to Install](#permissions-required-to-install) are present.
5. If any roles are missing, select your user/service account, and grant the roles using the **Grant Access** button. You may need to work with your administrator to be granted the correct roles.

### Step 2: Enable Required APIs

The APIs must be enabled at the project level.

To do so manually:

1. Click each of the API links in the table below.

2. Select the appropriate project and click **Enable**.

   ![](/image/gcp_api.png)

| API Name                                                                                                                         | API ID                                | Features                 | Projects                            | Usage                                                                                                                           |
|----------------------------------------------------------------------------------------------------------------------------------|---------------------------------------|--------------------------|-------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| [Identity and Access Management (IAM) API](https://console.cloud.google.com/marketplace/product/google/iam.googleapis.com)             | `iam.googleapis.com`                  | All Features             | All                                 | Used to access and collect IAM resources for CSPM and CIEM evaluations.                                                         |
| [IAM Service Account Credentials API](https://console.cloud.google.com/marketplace/product/google/iamcredentials.googleapis.com) | `iamcredentials.googleapis.com`       | All Features             | All                                 | Used to generate OAuth 2.0 access tokens.                                                                                       |
| [Security Token Service API](https://console.cloud.google.com/marketplace/product/google/sts.googleapis.com)                     | `sts.googleapis.com`                  | All Features             | All                                 | Used to exchange short-lived access tokens when interacting with Google Cloud resources.                                        |
| [Cloud Resource Manager API](https://console.cloud.google.com/marketplace/product/google/cloudresourcemanager.googleapis.com)    | `cloudresourcemanager.googleapis.com` | CSPM/CIEM                | All                                 | Used to gather resources such as organizations, projects, and IAM access control policy bindings for CSPM and CIEM evaluations. |
| [Cloud Identity API](https://console.cloud.google.com/apis/enableflow?apiid=cloudidentity.googleapis.com)                        | `cloudidentity.googleapis.com`        | CSPM/CIEM                | All                                 | Used to look up Google Group resource details.                                                                                  |
| [Admin SDK API](https://console.cloud.google.com/apis/enableflow?apiid=admin.googleapis.com)                                     | `admin.googleapis.com`                | CSPM/CIEM                | All                                 | Used to list users and their details, including information about the users who belong to Google Groups.                        |
| [Cloud Asset API](https://console.cloud.google.com/marketplace/product/google/cloudasset.googleapis.com)                         | `cloudasset.googleapis.com`           | CSPM/CIEM                | All                                 | Used to obtain a comprehensive inventory of Google Cloud resources for CSPM and CIEM evaluations                                |
| [Compute Engine API](https://console.cloud.google.com/apis/library/compute.googleapis.com)                                       | `compute.googleapis.com`              | Vulnerability Management/CSPM | All                            | Used by Vulnerability Management and CSPM to gather firewalls for network exposure analysis                                |
| [Organization Policy API](https://console.cloud.google.com/marketplace/details/google/orgpolicy.googleapis.com)                  | `orgpolicy.googleapis.com`            | CSPM                     | All                                 | Used by CSPM to retrieve organization policies for network exposure analysis                                                    |

#### Check API Enablement

To confirm that the required APIs were enabled:

1. Enable the `serviceusage.googleapis.com` Service API.

   This is required to execute the following command.

2. Execute: `gcloud services list --enabled`

​  All the services listed above should be included.

### Step 3: Authenticate and Configure Terraform

Configure your environment from your local machine, preparing to apply Terraform.

1. Ensure the prerequisites are met:

   - Terraform v.1.3.1+ installed
   - gcloud CLI installed

2. Authenticate your user and configure Terraform to use these credentials.

   A common way to do this is:

   1. Ensure you are logged in to the correct project.

      Log in using the GCP CLI:

      ```
      gcloud auth application-default login
      ```

      You will be presented with a web page to select your user account. Be sure to log in as the user you configured in Step 1.

   2. Confirm you are logged in as the correct user, by running the following and confirming that the expected user is active:

      ```
      gcloud auth list
      ```

For assistance, or instructions on alternative ways to authenticate Terraform, see the Terraform documentation: [Google Provider Configuration Reference](https://registry.terraform.io/providers/hashicorp/google/latest/docs/guides/provider_reference#authentication).

## Install using Wizard

1. Ensure you are authenticated to the GCP project you want to connect to in your terminal window. You can authenticate using the GCP CLI by running:

   `gcloud auth application-default login`.

2. Log in to Sysdig Secure as `admin`,  select **Integrations|Cloud Accounts > GCP**, and select **+Add GCP Account**.

   ![](/image/gcp-wizard2.png)

3. It is possible to install agentless **CDR only**.

4. In all other cases, all agentless AWS installations include **CSPM**.  
   All features are included by default. Deselect individual features if desired:

   - **Identity and Access (CIEM)**

   - **Cloud Detection and Response (CDR)**

   - **Vulnerability Host Scanning**

   and click **Next**.

5. Select which installation method matches your enterprise and click **Next**.

   - **Organization:** Configure GCP for an Organization
   - **Project:** Configure GCP for a single Project

The **Installation** screen appears.

### Installation

The entries on this page differ slightly depending on whether it's an [Organization](#organization) or [Project](#project) installation.  

#### Organization

1. As prompted by the wizard screen, specify the following:

   - **Organization Domain:** The domain of the GCP organization you are onboarding.
   - **Region of your GCP Project:** The region where resources will be created in your GCP project.
   - **Project ID:** The GCP project where the Sysdig resources will be deployed.

   The wizard will auto-populate a code snippet, along with autodetected Sysdig Secure endpoint and Sysdig Secure API token information.

2. Create a file called `main.tf`.
3. Copy the code snippet from the Wizard into the file and run `terraform init && terraform apply`.
4. (CSPM+CIEM only): Click **Next** in the wizard to set up [Domain-Wide Delegation](#domain-wide-delegation) in the Google Cloud Admin Console. Enabling DWD is optional and can be omitted if you don't want to provide those permissions to Sysdig.
5. After deploying, [validate the services are working](#validate).

#### Project

1. As prompted by the wizard screen, specify the following:

   - **Region of your GCP Project:**  The region where resources will be created in your GCP project.
   - **Project ID:** The ID of the GCP project that you are onboarding.

   The wizard will auto-populate a code snippet, along with autodetected Sysdig Secure endpoint and Sysdig Secure API token information.

2. Create a file called `main.tf`.

3. Copy the code snippet from the Wizard into the file and run `terraform init && terraform apply`.

4. (CSPM+CIEM only): Click **Next** in the wizard to set up [Domain-Wide Delegation](#domain-wide-delegation) in the Google Cloud Admin Console.  Enabling DWD is optional and can be omitted if you don't want to provide those permissions to Sysdig.

5. Configure domain-wide delegation.

    It allows a Google Workspace super admin to delegate authority to Sysdig service account to access user data on behalf of users within the domain.

6. After deploying, [validate the services are working](#validate).

### Configure Domain-Wide Delegation

#### What Is Domain-Wide-Delegation

In GCP, domain-wide delegation (DWD) refers to a feature in Google Workspace (formerly G Suite). It allows a Google Workspace super admin to delegate authority to a service account to access user data on behalf of users within the domain. Once set up, Sysdig uses a service account that can impersonate users by specifying the subject parameter in its authentication request, setting it to the email address of the Google Workspace user it wishes to impersonate.

Domain-wide delegation entails:

- **Service Account Access**: It allows a service account to impersonate a Google Workspace user and gain access to the Google data the user has access to, assuming they have provisioned the necessary Authorization scopes to the Service Account.
- **No User Consent Required**: With DWD, individual user consent is not required. Once the super admin sets up the delegation, the service account can access the specified data of any user in the domain without additional authorization prompts.
- **OAuth 2.0 Scopes**: When setting up DWD, the super admin specifies which OAuth 2.0 scopes the service account is granted. For instance, they might grant access to the Directory API to allow the service account to read group member data.
- **Security**: Because DWD grants broad access, it's essential to handle it with care. The service account's private key, which is used for authentication, should be kept secure.

#### Where it is Used

Sysdig's CIEM analysis requires DWD to provide:

- User and Group Insights derived from Google Workspace and Cloud Identity
  If DWD is enabled, then  *Unused Permission Criticality*, *Excessive Permissions*, and *Members* are displayed on the Identity and Access Groups page.
- Enhanced Monitoring and Reporting for MFA usage, user logins, admin console changes, and third-party application access
- Asset management to gain insights into Roles, Service Accounts, and their associated keys

The onboarding wizard prompts you to perform [domain-wide delegation](/en/gcp-domain-wide-delegation). If you skip this step, you will be prompted again from the **Identity and Access** (CIEM) page of the Sysdig Secure UI.

#### Enable Domain-Wide Delegation in GCP

##### Authorize Service Account Scopes

1. Log in to the [Google Admin Console](https://admin.google.com/) with Super Administrator privileges and select **Security > Access and data control > API controls**.

2. Click **Manage Domain Wide Delegation**.

3. Click **Add New**.

4. Switch to the [Google Cloud Console](https://console.cloud.google.com/) to collect your service account's **OAuth 2 Client ID**:

   - Navigate to the **Project** specified during the initial onboarding step.

   - Select **Service Account** and search for the newly created Sysdig service account with the format:  `sysdig-secure-a1b2@your-project-id.iam.gserviceaccount.com`.

   - Click the **Service Account** link to display the **OAuth 2 Client ID** and copy it.

     ![](/image/dwd_sa.png)

5. Return to the   [Google Admin Console](https://admin.google.com/) from Step 3.
   (**Security > Access and data control > API controls** **>** **Manage Domain Wide Delegation > Add New** ).

   ![](/image/dwd_add_client.png)

   In the panel, enter:

   - **Client ID:** Paste the OAuth 2 Client ID you copied.

   - **OAuth Scopes:** Add the OAuth scopes below in a comma-delimited list.

     ```
     https://www.googleapis.com/auth/cloud-identity.groups.readonly,
     https://www.googleapis.com/auth/admin.directory.user.readonly,
     https://www.googleapis.com/auth/admin.directory.group.readonly,
     https://www.googleapis.com/auth/admin.directory.group.member.readonly,
     https://www.googleapis.com/auth/cloud-platform.read-only,
     https://www.googleapis.com/auth/logging.read,
     https://www.googleapis.com/auth/admin.reports.audit.readonly,
     https://www.googleapis.com/auth/admin.reports.usage.readonly,
     
     ```

6. Click **Authorize**.

##### Create a Custom Admin Role and Grant Privileges

While still in the [Google Admin Console](https://admin.google.com/), go to **Account > Admin Roles**.

![](/image/dwd_create_role.png)

7. Click **Create new role**.

8. Enter the following values:

   - **Name:** Enter an appropriate name, such as `Secure Posture Management Read-Only Admin Role`.

   - **Description:** Optional

9. Click **Continue**. The **Select Privileges** page appears.

   ![](/image/dwd_console_priv.png)

10. Configure the Select Privileges as follows:

    - In **Admin Console Privileges**, at the top of the page, enable:

      - `Organization Units - Read`
      - `Users - Read`

    - Scroll down to **Admin API Privileges** and enable:

      - `Groups - Read`

    - Click **Continue**. Confirm the 5 privileges.

      ![](/image/dwd_review_priv.png)

    - Click **Create Role**. The **Admin Roles** screen appears.

      ![](/image/dwd_admin_roles.png)

11. Click **Assign Service Accounts**.

12. Enter the Sysdig service account name from step 4 and click **Add**.

    (Format: `sysdig-secure-a1b2@your-project-id.iam.gserviceaccount.com`)

    ![](/image/dwd_service_account.png)

13. A confirmation screen is displayed; click **Assign Role**.

#### Complete the Sysdig Onboarding Wizard

When all the enablement steps in GCP consoles are complete, return to the Sysdig wizard and click **Complete**.

![](/image/dwd_complete.png)

## Validate

Log in to Sysdig Secure and check that each module you deployed is functioning. It may take 10 minutes or so for events to be collected and displayed.

### Check Overall Connection Status

**Data Sources:** Select **Integrations > Data Sources | Cloud Accounts** to see all connected
cloud accounts.

### Check CSPM

**Inventory:** Select the **Inventory** module and filter for `project =`.  Check for your GCP cloud account in the drop-down.

### Check Identity and Access CIEM (Preview)

**Home:** Select **Home** and check the status bar at the top right to see your cloud accounts.

GCP resources are being rolled out for Identity and Access pages, starting with **Groups** page.

![](/image/home_status.png)

### Check Vulnerability Scanning

Check [Integrations > Data Sources | Cloud Hosts](/en/cloud-hosts).

## Features and Resources on GCP

### Agentless CSPM and Agentless CIEM

![](/image/agentless_gcp_diagram_cspm_ciem.png)

#### Resources Created

- `google_service_account`
- `google_service_account_key`
- `google_project_iam_member`
- `google_organization_iam_member` (Organizational Installs only)

### Agentless CDR

![](/image/agentless_gcp_diagram_cdr.png)

#### Resources Created

- `google_service_account`
- `google_service_account_iam_binding`
- `google_pubsub_topic`
- `google_pubsub_subscription`
- `google_pubsub_topic_iam_member`
- `google_project_iam_audit_config` (Single project installs only)
- `google_organization_iam_audit_config` (Organizational Installs only)
- `google_logging_project_sink` (Single project installs only)
- `google_logging_organization_sink` (Organizational Installs only)

#### Tune the Cloud Logs Volume

When you set up CDR, Sysdig creates a [log sink](https://cloud.google.com/logging/docs/export/configure_export_v2) to [access logs through Pub/Sub](https://cloud.google.com/logging/docs/export/pubsub#integrate-thru-pubsub). As a high volume of logs are costly to process, you may wish to tune the number of logs sent to the log sink:

- To tune which logs are generated, see [Tune Log Generation](#tune-log-generation).

- To set up inclusion or exclusion filters, see [Tune the Log Sink Filter](#tune-the-log-sink-filter).

 {{< callout type="info" >}}

  Tuning the configuration will change the logs that will be sent to Sysdig. This will affect Sysdig's runtime visibility and threat detection capabilities.

   {{% /callout %}}

##### Tune audit log generation

CDR enables [Data Access audit logs](https://cloud.google.com/logging/docs/audit#data-access) for every service on every connected project by default:

```
[
  {
    service = "allServices"
    log_config = [
      { log_type = "ADMIN_READ" },
      { log_type = "DATA_READ" },
      { log_type = "DATA_WRITE" }
    ]
  }
]
```

You can customize this to change the [enabled log types](https://cloud.google.com/logging/docs/audit/configure-data-access#configuration_overview) or [specify the single services](https://cloud.google.com/logging/docs/audit/configure-data-access#service-config). For a the list of services providing audit logs, see the [Google Cloud documentation](https://cloud.google.com/logging/docs/audit/services).

To customize this, specify the `audit_log_config` variable for the `-threat-detection` module (`single-project-threat-detection` or `organization-threat-detection`, depending on the setup). This example snippet enables only data writes for Cloud Storage and everything for Cloud SQL on a single project setup:

```
module "single-project-threat-detection" {
  ...
  audit_log_config = [
    {
      service = "storage.googleapis.com"
      log_config = [
        { log_type = "DATA_WRITE" }
      ]
    },
    {
      service = "cloudsql.googleapis.com"
      log_config = [
        { log_type = "ADMIN_READ" },
        { log_type = "DATA_READ" },
        { log_type = "DATA_WRITE" }
      ]
    }
  ]
}
```

{{< callout type="info" >}}

While the Terraform is unique to Sysdig, the configuration affects the project and the logs it generates, which could impact on consumers of those logs. If you want to keep generating these logs, without sending them to Sysdig, you can tune the log filter.

   {{% /callout %}}

##### Tune the Log Sink Filter

By default, the log sink filter is configured to only pick up Audit logs: `protoPayload.@type = \"type.googleapis.com/google.cloud.audit.AuditLog\"`. You can add exclusion filters to narrow the scope further.

You can customize the filter by specifying the `ingestion_sink_filter` in the `-threat-detection` module, `single-project-threat-detection` or `organization-threat-detection`, depending on the setup.

This example snippet excludes `get` and list `methods` for Data Access audit logs:

```
module "single-project-threat-detection" {
  ...
 ingestion_sink_filter = "protoPayload.@type = \"type.googleapis.com/google.cloud.audit.AuditLog\" (protoPayload.methodName !~ \"\\.(get|list)$\" OR logName !~ \"%2Fdata_access$\")"
}
```

Alternatively you can specify exclude filters with the `exclude_logs_filter` variable in the same module.

This example excludes:

- gets and lists on kubernetes
- gets and lists on Google Cloud Storage (GCS)

```
module "single-project-threat-detection" {
  ...
  exclude_logs_filter = [
    {
      name        = "k8s_get_list"
      description = "Exclude get and list methods on kubernetes"
      filter      = "protoPayload.serviceName=\"k8s.io\" AND protoPayload.methodName =~ (\"\\.get$\" OR \"\\.list$\")"
    },
    {
      name        = "storage_objects_get_list"
      description = "Exclude get and list methods on GCS objects"
      filter      = "protoPayload.methodName = (\"google.storage.objects.get\" OR \"google.storage.objects.list\" OR \"storage.objects.get\" OR \"storage.objects.list\")"
    }
  ]
}
```

The following example for the organization threat detection module excludes:
- specific project logs
- specific folder logs

```
module "organization-threat-detection" {
  ...
  exclude_logs_filter = [
    {
      name        = "exclude_project_test"
      description = "Exclude GCP Test Project logs"
      filter      = "source(projects/my-test-project-id)"
    },
    {
      name        = "exclude_folder_sandbox"
      description = "Exclude GCP Sandbox Folder logs"
      filter      = "source(folders/my-sandbox-folder-id)"
    },
  ]
}
```

For the full syntax of the filtering options, see [Google Cloud Logging query language page](https://cloud.google.com/logging/docs/view/logging-query-language).

### Vulnerability Management Agentless Host Scanning

![](/image/agentless_gcp_cloud_scan.png)

#### Resources Created

**Note:** `google_project` entries are used for single projects; replace with `google_organization` for organizaations.

- `google_iam_workload_identity_pool`
- `google_iam_workload_identity_pool_provider`
- `google_project_iam_custom_role`
- `google_project_iam_binding`
- `google_service_account`
- `google_service_account_iam_member` x2, to bind service_account to a custom and existing role

Host Scanning authentication involves fetching the opted-in instance disks:

- `google_project_iam_binding`
- `google_project_iam_custom_role`

See also the  [VM Host Scanning Terraform Readme](https://github.com/sysdiglabs/terraform-google-secure/tree/master/modules/services/agentless-scan).

#### How to Exclude/Include Resources from Vulnerability Scanning

When you connect a GCP project with Vulnerability Host Scanning, by default all Compute Instances with root volumes in the project are included in the scan.

If you need to exclude specific Compute Instances from being scanned, you can do so using tags.

**How to exclude Instances:** To exclude certain Compute Instances from being scanned, you must assign specific tags to them in the GCP Console or using GCP APIs.

It is recommended to set these tags before initiating the scanning process.
You can add tags after onboarding, but note that the exclusion will only take effect in subsequent scans.

**How to include Data Volumes in Scans:** By default, only root volumes of Compute Instances are scanned.

To also include data volumes in scans, you need to use the specific tags declared below.

#### Tagging Semantics

You can use the following tags at Volume or Compute Instance level.
Tagging can be added at any time, for example, if you want to exclude/include something that was or was not scanned.

**Key:** `sysdig-secure-scan`, `sysdig-secure-data-volumes-scan`.

**Values:** `true`, `false`

**Usage Examples**

- `“sysdig-secure-scan” : “false”` on a Compute Instance excludes the instance and all its volumes from scanning;
- `“sysdig-secure-scan” : “true”` on a data-volume of a Compute Instance includes such volume for scanning;
- `“sysdig-secure-data-volumes-scan” : “true”` on a Compute Instance has the same effect as applying the `“sysdig-secure-scan” : “true”` tag to all its data-volumes.

The following tags are redundant; using them will have the same effect as not having them.
This is either because Sysdig scans them by default or because the values have been overridden by a tag at a higher level (such as a Compute Instance).

- `“sysdig-secure-scan” : “true”` on a Compute Instance;
- `“sysdig-secure-scan” : “true”` on the root volume of a Compute Instance;
- `“sysdig-secure-scan” : “false”` on any data-volumes of a Compute Instance;
- `“sysdig-secure-scan” : “false”` on the root volume of a Compute Instance has no effect. The root volume is always scanned as part of the Compute Instance scan;
- `“sysdig-secure-data-volumes-scan” : “false”` on a Compute Instance;
- `“sysdig-secure-data-volumes-scan” : “false”` on any data-volumes of a Compute Instance.