---
linkTitle: "Forwarding to IBM MCM"
title: "Forwarding to IBM MCM"
weight: "16"
aliases:
  - /en/forwarding-to-ibm-cloud-pak-for-multicloud-management.html
description: "IBM Cloud Pak for Multicloud Management (MCM) centralizes visibility, governance, and automation for containerized workloads across clusters and clouds into a single dashboard. This page describes how to forward events to IBM MCM in Sysdig Secure."
---
There are two ways to integrate IBM MCM with Sysdig Secure: through the UI, or through Agent Local Fowarding. Sysdig recommends the former method, but both are covered on this page.

## Prerequisites

* Generate a Grafeas [API key](https://cloud.ibm.com/docs/account?topic=account-serviceidapikeys&interface=ui) and authenticate an [IBM Cloud IAM token](https://cloud.ibm.com/docs/account?topic=account-iamtoken_from_apikey).

## Integrate IBM MCM through the UI

Event data can be forwarded to IBM Cloud Pak for Multicloud Management. Here's how to set up an integration through the UI:

1. Log in to Sysdig Secure as an Admin.

3. From **Integrations** or **Settings**, navigate to the **Events Forwarding** tab.

2. Click the **Add Integration** button.

3. Select **IBM MCM** from the drop-down.

4. Configure the required options:

    ![](/image/ibm_mcm.png)

    **Integration Name:** Define an integration name.

    **URL:** This is the URL for your IBM MCM API endpoint. This should be the same that you use to connect to the IBM MCM CloudPak console. Use the format `scheme://host:port`.

    **Grafeas API Key:** Enter your IBM [API key](https://cloud.ibm.com/docs/account?topic=account-serviceidapikeys&interface=ui).

    **Account ID:** (Optional) You can leave it blank to use the default value of `id-mycluster-account`. If you want to use a different account name, provide it here.

    **Data to Send:** Select from the drop-down the types of Sysdig data that should be forwarded. The available list depends on the Sysidg features and products you have enabled.

    **Allow insecure connnections**: Click the box if you want to skip TLS certificate verification and allow insecure connections.

    **Enabled**: Toggle to enable the integration. You will need to successfully **Test Integration** with the button below before the integration can be enabled.

5. Click **Save**.

## Integrate IBM MCM through Agent Local Forwarding

If you wish to integrate IBM MCM through Agent Local Fowarding, review the [configuration steps](/en/event-forwarding/#configure-agent-local-forwarding) and use the following parameters for this integration:

| **Attribute**   | **Required?** | **Type**             | **Default** | **Description**                                              |
| -------- | --------------- | ------------- | -------------------- | ------------------------------------------------ | ----------- | ------------------------------------------------------------ |
| endpoint        | yes           | string               |              | The URL, including protocol and port (if non standard), to your IBM Cloud Pak for Multicloud Management API endpoint    |
| apiKey | yes         | string               |                              | IBM Cloud API Key |
| insecure        | no            | bool                 | false       | Skip TLS certificate verification            |
| accountId    | no            | string               |  id-mycluster-account        | Account ID to use on MCM           |
| providerId      | no            | string |   sysdig-secure          | The provider the findings will be associated with |
| noteName       | no            | string               | policy-event   | The note to use. If unspecified, a note with `policy-event` ID will be created and used.                                             |
