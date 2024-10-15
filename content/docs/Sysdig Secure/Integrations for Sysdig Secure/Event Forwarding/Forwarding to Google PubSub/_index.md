---
linkTitle: "Forwarding to Google PubSub"
title: "Forwarding to Google PubSub"
weight: "18"
aliases:
  - /en/forwarding-to-google-pubsub.html
  - /en/forward-pubsub
description: "Google Pub/Sub allows services to communicate asynchronously and is used for streaming analytics and data integration pipelines to ingest and distribute data. It is equally effective as messaging-oriented middleware for service integration or as a queue to parallelize tasks. See [Common Use Cases](https://cloud.google.com/pubsub/docs/overview) for more background details."
---

## Prerequisites

Event forwards originate from region-specific IPs. For the full list of outbound IPs by region, see [SaaS Regions and IP Ranges](/en/docs/administration/saas-regions-and-ip-ranges/#saas-regions-and-ip-ranges).  Update your firewall and allow inbound requests from these IP addresses
to enable Sysdig to handle event forwarding.

**NOTE:** The permissions for the service account must be either `Editor` or `Admin.` `Publisher` is not sufficient.

## Configure Standard Integration

1. Log in to Sysdig Secure as Admin and go to **Profile > Settings > Event Forwarding**.

2. Click **+Add Integration** and choose **Google Pub/Sub**  from the drop-down menu.

3. Configure the required options:

* **Integration Name:** Define an integration name.

* **Project:** Enter the Cloud Console project name you created in Google Pub/Sub.

* **Topic:** Enter the Topic Name you created.

* **JSON Credentials:** Enter the Service Account credentials you created.

* **Attributes:** If you have chosen to embed custom attributes as metadata in Pub/Sub messages, enter them here.

* **Ordering Key:** If you chose to have subscribers receive messages in order, enter the ordering key information you set up.

* **Data to Send:** Select from the drop-down the types of Sysdig data that should be forwarded. The available list depends on the Sysdig features and products you have enabled.

* Toggle the enable switch as necessary. Remember that you will need to "Test Integration" with the button below before enabling the integration.

5. Click **Save**.

## Configure Agent Local Forwarding

Review the [configuration steps](/en/event-forwarding/#configure-agent-local-forwarding) and use the following parameters for this integration.

| **Type** | **Attribute**   | **Required?** | **Type**             | **Allowed values** | **Default** | **Description**                                              |
| -------- | --------------- | ------------- | -------------------- | ------------------ | ----------- | ------------------------------------------------------------ |
| PUBSUB   | project         | yes           | string               |                    |             | project hosting the target pub/sub                           |
| PUBSUB   | topic           | yes           | string               |                    |             | pub/sub topic onto which publish the data                    |
| PUBSUB   | credentialsJSON | yes           | string               |                    |             | credentials JSON file content used to authenticate as a service account in the project |
| PUBSUB   | attributes      | no            | sequence of mappings |                    |             | Extra headers to add to the request. Each header mapping requires 2 keys: “key” for the header key and “value” for its value |
| PUBSUB   | orderingKey     | no            | string               |                    |             | The key to use to order the messages. Required to enable ordered delivery |
