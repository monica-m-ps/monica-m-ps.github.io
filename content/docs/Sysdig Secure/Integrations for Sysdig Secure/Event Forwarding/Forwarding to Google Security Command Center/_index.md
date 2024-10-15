---
linkTitle: "Forwarding to Google Security Command Center"
Title: "Forwarding to Google Security Command Center"
weight: "25"
aliases:
  - /en/forwarding-to-google-security-command-center.html
  - /en/forward-scc
description: "[Google Security Command Center](https://cloud.google.com/security-command-center/docs) or SCC is a centralized vulnerability and threat reporting service that helps strengthen your security posture and provide asset inventory and discovery."
---

{{% callout type="note" title="Supported data" %}}
At this time, only GCP Audit Log events can be forwarded to this integration.
{{% /callout %}}

## Prerequisites

1. **Handle region-specific items:** Event forwarders originate from region-specific IPs. For the full list of outbound IPs by region, see [SaaS Regions and IP Ranges](/en/docs/administration/saas-regions-and-ip-ranges/#saas-regions-and-ip-ranges).  Update your firewall and allow inbound requests from these IP addresses to enable Sysdig to handle event forwarding.

2. **Enable integration from [GCP console](https://console.cloud.google.com/apis/library):**  Select *Enable APIs and Services*, and enable the following APIs:

     * Security Command Center API
     * Identity and Access Management (IAM) API

3. **Handle Service Account:** A service account with the right permissions is required. The following example illustrates how to do it automatically from the terminal. The values `PROJECT_ID` and `ORG_ID` must be provided. `SERVICE_ACCOUNT` refers to the desired name for the account. `KEY_LOCATION` refers to the desired name for the JSON output file that will need to be uploaded into the Sysdig UI in the next step.

    ```
      export SERVICE_ACCOUNT=scc-servaccount
      export PROJECT_ID=elevated-web-872901
      export KEY_LOCATION=scckey.json
      export ORG_ID=494436833222
    
      gcloud iam service-accounts create $SERVICE_ACCOUNT  \
        --display-name "Service Account for USER"  \
        --project $PROJECT_ID
    
      gcloud iam service-accounts keys create $KEY_LOCATION  \
        --iam-account $SERVICE_ACCOUNT@$PROJECT_ID.iam.gserviceaccount.com
    
      gcloud beta organizations add-iam-policy-binding $ORG_ID \
        --member="serviceAccount:$SERVICE_ACCOUNT@$PROJECT_ID.iam.gserviceaccount.com" \
        --role='roles/securitycenter.admin'
    ```

## Configure Standard Integration

To forward event data to Google SCC:

1. Log in to Sysdig Secure as Admin and go to **Profile > Settings > Event Forwarding**.
2. Click **+Add Integration** and choose **Google SCC** from the drop-down menu.
3. Configure the required options:

* **Integration Name:** Define an integration name.
* **Organization:** Set the ID of your GCP organization.
* **JSON credentials:** Updload JSON credentials that you previously generated from a service account or user.
* **Data to Send:** Select from the drop-down the types of Sysdig data that should be forwarded. Note that since only GCP Audit Log events can be forwarded, only Runtime Policy events are shown.
  * Toggle the enable switch as necessary. Remember that you will need to "Test Integration" with the button below before enabling the integration.

5. Click **Save**.

## Configure Agent Local Forwarding

Review the [configuration steps](/en/event-forwarding/#configure-agent-local-forwarding) and use the following parameters for this integration.

| **Type** | **Attribute**   | **Required?** | **Type** | **Allowed values** | **Default** | **Description**                                              |
| -------- | --------------- | ------------- | -------- | ------------------ | ----------- | ------------------------------------------------------------ |
| SCC      | organization    | yes           | string   |                    |             | GCP organization ID                                          |
|          |                 |               |          |                    |             |                                                              |
| SCC      | credentialsJSON | yes           | string   |                    |             | credentials JSON file content used to authenticate as a service account in the project |
