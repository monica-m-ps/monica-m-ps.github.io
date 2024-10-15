---
linkTitle: "Legacy"
title: "(Legacy) Agent-Based Threat Detection"
weight: "11"
no_list: true
aliases:
  - /en/docs/installation/sysdig-secure-for-cloud/deploy-sysdig-secure-for-cloud-on-gcp/
  - /en/docs/sysdig-secure/sysdig-secure-for-cloud/gcp
  - /en/gcp-secureagent-based-threat-detection/
  - /en/gcp-secure-agent-threat
description: "This page describes the legacy process to install agent-based threat detection/CDR on GCP, for single projects or organizations." 
---

## Prerequisites and Permissions

- A **Sysdig Secure SaaS account**, with administrator permissions.
- Have **Terraform** [installed](https://learn.hashicorp.com/tutorials/terraform/install-cli) on the machine from which you will deploy the installation code.
  - [Terraform Google Platform Provider](https://registry.terraform.io/providers/hashicorp/google/latest/docs).
  - Google's [Cloud SDK](https://cloud.google.com/sdk/docs/install) deployed in the environment where you will deploy the installation code.
- A **Google Cloud Platform (GCP) account**, for Sysdig Secure's cloud compute workload deployment, with the following roles and permissions required to install.
  - _Owner_ role, in order to create each of the resources specified in the resources list below.
  - For organizational _Organization Admin_ role is required too.
  - Check that the following [_GCP Service APIs_](https://registry.terraform.io/modules/sysdiglabs/secure-for-cloud/google/latest#prerequisites) are enabled:
    - For _Agentless CSPM_:
      - [Identity and access management API](https://console.cloud.google.com/marketplace/product/google/iam.googleapis.com) (`iam.googleapis.com`)
      - [IAM Service Account Credentials API](https://console.cloud.google.com/marketplace/product/google/iamcredentials.googleapis.com)(`iamcredentials.googleapis.com`)
      - [Cloud Resource Manager API](https://console.cloud.google.com/marketplace/product/google/cloudresourcemanager.googleapis.com)(`cloudresourcemanager.googleapis.com`)
      - [Security Token Service API](https://console.cloud.google.com/marketplace/product/google/sts.googleapis.com) (`sts.googleapis.com`)
      - [Cloud Asset API](https://console.cloud.google.com/marketplace/product/google/cloudasset.googleapis.com) (`cloudasset.googleapis.com`)

    See [Permissions Granted](#permissions-granted) for the roles and permissions used by the installed modules after installation.

#### Gather the Following

- [Sysdig Secure endpoint](/en/saas-regions/) (by region)
- [Sysdig API token](/en/api-token/)
- `GCP Region` for example, `us-east1` The region where resources will be created in your GCP project by default.
- `Project ID` of the project in which compute resources will be deployed.
- `Organization Domain` (If performing an Organizational deploy).

#### Check API Enablement

To check that all the required _GCP Service APIs_ are enabled execute:

```
gcloud services list --enabled | grep -E '(iam.googleapis.com|iamcredentials.googleapis.com|cloudresourcemanager.googleapis.com|sts.googleapis.com|cloudasset.googleapis.com|recommender.googleapis.com|cloudidentity.googleapis.com|admin.googleapis.com)'
```

All the services listed above should be included.
_Note that you need to enable the `serviceusage.googleapis.com` Service API to use this command._

#### Available Options

**Workload Types:** `Cloudrun`, `Kubernetes`

[Check example input parameters](https://registry.terraform.io/modules/sysdiglabs/secure-for-cloud/google/latest) for these and other configuration options.

## Install Agent-Based Threat Detection

{{< callout type="info" >}}

This method installs ONLY  [Threat Detection](/en/threat-policies/). Use the Wizard to obtain agentless CSPM  [Compliance](/en/compliance/) and agentless CIEM [Identity and Access](/en/ia-ciem/) for GCP.

{{% /callout %}}

This installation is manual and can be performed for a single project or organizational project in Terraform.

### Single Project

1. In a terminal window, ensure you are authenticated to the GCP project you would like to connect.
    You can authenticate using the GCP CLI by running `gcloud auth application-default login`

2. Save the following to a file named `main.tf` on your local machine:

    ```terraform
    terraform {
      required_providers {
        sysdig = {
          source = "sysdiglabs/sysdig"
        }
      }
    }
    
    provider "sysdig" {
      sysdig_secure_url       = "<SYSDIG_SECURE_URL>"
      sysdig_secure_api_token = "<SYSDIG_SECURE_API_TOKEN>"
    }
    
    provider "google" {
      project = "<PROJECT_ID>"
      region  = "<GCP_REGION>"
    }
    
    provider "google-beta" {
      project = "<PROJECT_ID>"
      region  = "<GCP_REGION>"
    }
    
    module "single-project" {
      source           = "sysdiglabs/secure-for-cloud/google//examples/single-project"
      deploy_benchmark = false
    }
    ```

3. Replace the following placeholders in `main.tf`:
   - `SYSDIG_URL`: Use the endpoint for the region in which your Sysdig Secure platform is installed:
     - US East: `https://secure.sysdig.com`.
     - US West: `https://us2.app.sysdig.com`
     - European Union:`https://eu1.app.sysdig.com` <br/>
   - `SYSDIG_API_TOKEN`: See [Retrieve the Sysdig API Token](/en/docs/developer-tools/sysdig-python-sdk/#sysdig-python-sdk#retrieve-the-sysdig-api-token) to find yours.
   - `GCP_REGION`: e.g. `us-east1` The region where resources will be created in your GCP project by default.
   - `GCP_PROJECT_ID`: The GCP Project ID that you are onboarding.

4. Run `terraform init && terraform apply`.

6. After deploying, [confirm that Threat Detection is working](#validate).

### Organization

1. In a terminal window, ensure you are authenticated to the GCP project in which you would like to set up [Identity Federation](https://cloud.google.com/iam/docs/workload-identity-federation).
   You can authenticate using the GCP CLI by running `gcloud auth application-default login`

2. Create a file called `sysdig.tf` with the following contents:

    ```terraform
    terraform {
      required_providers {
        sysdig = {
          source = "sysdiglabs/sysdig"
        }
      }
    }
    
    provider "sysdig" {
      sysdig_secure_url       = "<SYSDIG_SECURE_URL>"
      sysdig_secure_api_token = "<SYSDIG_SECURE_API_TOKEN>"
    }
    
    provider "google" {
      project = "<PROJECT_ID>"
      region  = "<GCP_REGION>"
    }
    
    provider "google" {
      alias  = "multiproject"
      region = "<GCP_REGION>"
    }
    
    provider "google-beta" {
      alias  = "multiproject"
      region = "<GCP_REGION>"
    }
    
    module "organization" {
      providers = {
        google.multiproject      = google.multiproject
        google-beta.multiproject = google-beta.multiproject
      }
      source = "sysdiglabs/secure-for-cloud/google//examples/organization-org_compliance"
    
      organization_domain = "<ORGANIZATION_DOMAIN>"
      deploy_benchmark    = false
    }
    ```

3. Replace the following placeholders in `main.tf`:
    - `SYSDIG_URL`: Use the endpoint for the region in which your Sysdig Secure platform is installed:
        - US East: `https://secure.sysdig.com`.
        - US West: `https://us2.app.sysdig.com`
        - European Union:`https://eu1.app.sysdig.com` <br/>
    - `SYSDIG_API_TOKEN`: See [Retrieve the Sysdig API Token](/en/docs/developer-tools/sysdig-python-sdk/#sysdig-python-sdk#retrieve-the-sysdig-api-token) to find yours.
    - `GCP_PROJECT_ID`: The GCP Project ID where [Identity Federation](https://cloud.google.com/iam/docs/workload-identity-federation) resources will be created.
    - `GCP_REGION`: e.g. `us-east1` The region where resources will be created in your GCP project by default.
    - `GCP_ORG_DOMAIN`: The domain of the GCP organization you are onboarding.

4. Run `terraform init`.

5. Run `terraform apply`.

6. After deploying, [confirm that Threat Detection is working](#validate).

## Validate

Log in to Sysdig Secure and check the module you deployed is
functioning. It may take 10 minutes or so for events to be collected and
displayed.

### Check Overall Connection Status

- **[Data Sources](/en/docs/sysdig-secure/integrations-for-sysdig-secure/data-sources/cloud-accounts):** Select **Integrations > Data Sources | Cloud Accounts** to see all connected
  cloud accounts.

### Check Threat Detection

- **[Policies and Rules](/en/docs/sysdig-secure/policies/threat-detect-policies/)**:  Check `Policies > Runtime Policies` and confirm that
  the `Sysdig AWS Threat Detection` and `Sysdig AWS Threat Intelligence` managed policies are enabled.

  - These consist of the most-frequently-recommended rules for AWS and CloudTrail. You can
      customize them by creating a new policy of the [AWS CloudTrail](/en/docs/sysdig-secure/policies/manage-policies/#select-the-policy-type)
      type.
- **[Events:](/en/events-secure/)** In the Events feed, filter for `aws.accountid =` and check for your cloud account.
- **Force an event**: To manually create an event, choose one of the rules contained an AWS policy and  execute it in your AWS account.
  <br/>ex.: Create a S3 Bucket with Public Access Blocked. Make it public to prompt the event.
  <br/>Remember that  new rules added to policies require time to propagate the changes.

## Features and Resources Created

### [Threat Detection/CDR](/en/cloud-accounts-secure/)

#### Resources Created

- google_cloud_run_service
- google_cloud_run_service_iam_member
- google_eventarc_trigger
- google_logging_organization_sink
- google_logging_project_sink
- google_project_iam_member
- google_pubsub_subscription
- google_pubsub_subscription_iam_member
- google_pubsub_topic
- google_pubsub_topic_iam_member
- google_secret_manager_secret
- google_secret_manager_secret_iam_member
- google_secret_manager_secret_version
- google_service_account
- google_service_account_iam_binding

## Permissions Granted

`roles/eventarc.eventReceiver`

`roles/iam.serviceAccountTokenCreator`

`roles/secretmanager.secretAccessor`

`roles/pubsub.subscriber`

`roles/pubsub.publisher`

`roles/run.invoker`

`roles/run.viewer`

`roles/cloudbuild.builds.builder` (If scanning enabled)

`roles/iam.serviceAccountUser` (If scanning enabled)

`customRole` (If organizational)

- `storage.objects.get`
- `storage.objects.list`
- `artifactregistry.repositories.get`
- `artifactregistry.repositories.downloadArtifacts`
- `artifactregistry.tags.list`
- `artifactregistry.tags.get`
- `run.services.get`

## Next Steps

If you want to add CSPM and Identity and Access/CIEM features, run the [Agentless](/en/gcp-secure-agentless) installation after the manual installation.
