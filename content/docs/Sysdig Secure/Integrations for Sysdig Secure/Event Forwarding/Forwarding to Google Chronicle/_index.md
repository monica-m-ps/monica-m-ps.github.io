---
linkTitle: "Forwarding to Google Chronicle"
title: "Forwarding to Google Chronicle"
weight: "10"
aliases:
  - /en/forwarding-to-google-chronicle.html
description: "Sysdig supports forwarding events to [Chronicle](https://cloud.google.com/chronicle/docs), Google Cloud Platform's centralized interface for aggregating security tools, logs, and more. This page describes how to forward data such as events, platform audit, and activity audit from Sysdig to Chronicle."
---

Event Forwarding to Chronicle is authenticated with a Service Account. The legacy authentication method, involving an API Key, continues to be supported. However, we recommend that you use Service Accounts for authentication. See [Service Accounts](/en/ciem-service-accounts).

## Prerequisites

Event forwards originate from region-specific IPs. For the full list of outbound IPs by region, see [SaaS Regions and IP Ranges](/en/docs/administration/saas-regions-and-ip-ranges/#saas-regions-and-ip-ranges).  Update your firewall and allow inbound requests from these IP addresses to enable Sysdig to handle event forwarding.

{{% callout type="note" %}}

Google Chronicle v2 now uses JSON format, which Sysdig does currently support. Contact Google Chronicle customer support to request a v1 API key.

{{% /callout %}}

## Configure Event Forwarding

To set up Event Forwarding to Chronicle with a Service Account:

1. Log in to Sysdig Secure as Admin.

2. Open **Settings** > **Event Forwarding**. Alternatively, **Integrations** > **Event Forwarding**.

3. From the top right corner, select **Add Integration** and **Google Chronicle**.

4. Specify the following:

    ![](/image/chronicle_forwarding.png)
  
    - **Integration Name**: A unique name to help you identify the Chronicle integration.

    - **Customer ID**: The Google Customer ID associated with your GCP account. In the Google Chronicle UI, find this in **Settings > Profile > IDP USER ID**.

     ![](/image/chronicle_id.png)

    - **Namespace**: User-configured environment namespace to identify the data domain the logs originated from. Use namespace as a tag to identify the appropriate data domain for indexing and enrichment functionality.

    - **JSON Credentials**: Upload your Google Chronicle JSON credentials. See [Getting API Authentication Credentials](https://cloud.google.com/chronicle/docs/reference/ingestion-api#getting_api_authentication_credentials).

    - **Region**: Select your region, such as US, Europe, or Asia.

    - **Data to Send**: From the drop-down, select which data to forward, such as activity audit, Sysdig platform audit, and runtime policy events. The available list depends on the Sysdig features and products you have enabled.

5. Test the integration, then toggle **Enabled** to activate it.

6. Click **Save** to finish.

## Configure Agent Local Forwarding

Review the [configuration steps](/en/event-forwarding/#configure-agent-local-forwarding) and use the following parameters for this integration.

| **Type**  | **Attribute**       | **Required?** | **Type** | **Allowed values**          | **Default** | **Description**                           |
|-----------|---------------------|---------------|----------|-----------------------------|-------------|-------------------------------------------|
| CHRONICLE | credentialsOAuth2   | yes           | string   |                             |             | The Goolge Chronicle JSON credentials     |
| CHRONICLE | region              | no            | string   | us, europe, asia-southeast1 | us          | The target region                         |
| CHRONICLE | chronicleCustomerId | yes           | string   |                             |             | The Google Chronicle Customer ID          |
| CHRONICLE | namespace           | yes           | string   |                             |             | The namespace to identify the data domain |
