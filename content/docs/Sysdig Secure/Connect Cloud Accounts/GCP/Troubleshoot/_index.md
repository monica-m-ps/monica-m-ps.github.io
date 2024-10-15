---
linkTitle: "Troubleshoot"
title: "Troubleshoot GCP Agentless Installs"
weight: "10"
no_list: true
aliases:
  - /en/troubleshoot-gcp-agentless
description: "Use the following steps to troubleshoot agentless GCP integration issues."

---

## Check for Workload Identity Federation Configuration

Misconfigured [GCP Workload Identity Federation](https://console.cloud.google.com/iam-admin/workload-identity-pools) (WIFs) can commonly hinder Sysdig's operation by denying required permissions. To check for WIFs that may impact Sysdig Integrations (replace `PROJECTID` and `PROJECTNUMBER` as needed):

1. Log into GCP console and select the affected project in [the homepage](https://console.cloud.google.com).
2. In [Workload Identity Pool](https://console.cloud.google.com/iam-admin/workload-identity-pools), the associated Workload Identity Pool provider that's configured must have an ID with the prefix `sysdig-*`. 

   You can choose any display name.

3. The [configured pool](https://console.cloud.google.com/iam-admin/workload-identity-pools/pool/sysdigcloud) should have a connected service account with the name prefix `sysdig-*`. This was was configured when the account was created. The service account should have the email `sysdig-*@PROJECTID.iam.gserviceaccount.com`.
4. This service account should allow access to the following principal set:

   - **For webhook-datasource:**
     `principalSet://iam.googleapis.com/projects/PROJECTNUMBER/locations/global/workloadIdentityPools/sysdig-*/attribute.aws_role/arn:aws:sts::263844535661:assumed-role/us-west-2-production-secure-assume-role/77135e36ab5102091c579abfd9eab3a5`

   - **For agentless-scan and workload-scan with AWS Sysdig Backend:**
     `principalSet://iam.googleapis.com/sysdig-*/attribute.aws_account/<sysdig backend AWS account id>`

   - **For agentless-scan and workload-scan with GCP Sysdig Backend:**
     `principalSet://iam.googleapis.com/sysdig-*/attribute.sa_id/<sysdig backend GCP account id>`

5. The service account should have either the `iam.serviceAccountTokenCreator` role or more specifically `iam.serviceAccounts.getAccessToken` role, as well as `iam.workloadIdentityUser` role on the target project. For agentless-scan, it should have a custom role containing the host discovery and host scan related permissions.
6. The [pool provider](https://console.cloud.google.com/iam-admin/workload-identity-pools/pool/sysdigcloud/provider/sysdigcloud) should allow access to the AWS account ID: `263844535661`. This is Sysdig's trusted identity and can be retrieved with `curl --location --request GET 'https://us2.app.sysdig.com/api/cloud/v2/gcp/trustedIdentity`.

   For scanning, such as agentless-scan or workload-scan using the GCP Backend, allow access to the GCP account ID.

## Troubleshoot Agentless CSPM and Identity
- Ensure the service account created in the affected account contains the following roles:
  - `browser` role.
  - `iam.serviceAccountTokenCreator` , `cloudasset.viewer`, `logging.viewer`, `cloudfunctions.viewer` and `cloudbuild.builds.viewer` roles.
- For identity management, ensure the service account has the following roles attached:
  - `iam.serviceAccountViewer`, `recommender.viewer`, `iam.roleViewer`, `container.clusterViewer` and `compute.viewer` roles.
- Ensure the service account has a key created and it is enabled.

## Troubleshoot Agentless CDR
- **Ingestion resources:** Ensure the affected account has a pubsub topic (named `ingestion_topic`), an associated project sink and a push subscription created. (prefixed with `ingestion_topic`). Organizational installations will have organization sink.
- Ensure the project/organization has audit logs configured to be sent to the pubsub topic.
- Ensure the pubsub topic has `pubsub.publisher` role attached to publish the ingestion logs.
- Ensure that the Cloud Pub/Sub API has been enabled in the management project.
- Ensure the push subscription has the correct push endpoint configured.
- Ensure that the Service Agent for the subscription has been provisioned correctly.

    This can be verified by running the following `gcloud` command (replace `<PROJECT_ID>` with your management project ID):
    ```
    gcloud projects get-iam-policy <PROJECT_ID> --flatten="bindings[].members" --format='table(bindings.role, bindings.members)' | grep @gcp-sa-pubsub.iam.gserviceaccount.com
    ```
    It returns either of these 2 lines:
    ```
    roles/pubsub.serviceAgent serviceAccount:service-<PROJECT_ID_NUMBER>@gcp-sa-pubsub.iam.gserviceaccount.com
    roles/iam.serviceAccountTokenCreator serviceAccount:service-<PROJECT_ID_NUMBER>@gcp-sa-pubsub.iam.gserviceaccount.com
    ```
    If no line is returned:
  1. Disable and enable the Cloud Pub/Sub API in your management project and try again.
  2. If it's still not working, from your GCP console:
      1. Open the Subscriptions page on the Pub/Sub service
      2. Select `ingestion_topic_push_subscription`
      3. If the service agent doesn't have the right role, you'll be prompted to assign it through a message. Please grant it.
  3. Ensure the Sysdig Log ingestion Service Account (starting with `sysdig-ingestion-`) has been deployed and the role `roles/iam.serviceAccountTokenCreator` has been assigned to it.

## Troubleshoot Agentless Vulnerability Scanning
- To discover compute Virtual Private Cloud (VPC)/Instance/Volume resources, ensure the service account created in the affected account has the host discovery permissions attached.
- To discover compute zone operations and disks resource, ensure the service account created in the affected account has the host scan permissions attached.
- If certain resources (such as compute instances / volumes) are not being scanned, ensure those resources don't have `sysdig-secure-scan`/`sysdig-secure-data-volumes-scan` tags set to `false`.
