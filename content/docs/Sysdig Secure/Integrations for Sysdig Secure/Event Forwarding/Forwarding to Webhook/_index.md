---
linkTitle: "Forwarding to Webhook"
title: "Forwarding to Webhook"
weight: "40"
aliases:
  - /en/forwarding-to-webhook.html
  - /en/forward-webhook
description: "Webhooks are user-defined HTTP callbacks. They are usually triggered by an event. When that event occurs, the source site makes an HTTP request to the URL configured for the webhook. Users can configure webhooks to cause events on one site to invoke behavior on another."
---
Sysdig Secure leverages webhooks to support integrations that are not covered by any other particular integration/protocol present in the Event Forwarder list.

## Prerequisites

Event forwards originate from region-specific IPs. For the full list of outbound IPs by region, see [SaaS Regions and IP Ranges](/en/docs/administration/saas-regions-and-ip-ranges/#saas-regions-and-ip-ranges).  Update your firewall and allow inbound requests from these IP addresses
to enable Sysdig to handle event forwarding.

## Configure Standard Integration

To forward secure data to a Webhook:

1. Log in to Sysdig Secure as Admin and go to **Profile > Settings > Event Forwarding**.

2. Click **+Add Integration** and choose **Webhook** from the drop-down menu.

3. Configure the required options:

   **Integration Name:** Define an integration name.

   **Endpoint**: Webhook endpoint following the schema protocol
   (i.e.`https://)hostname:port`

   **Authentication**: Four different methods are supported:

   - **Basic authentication:** If you select this method, you must
       fill the `Secret` field with the desired `user: password`. No
       whiteespaces, semicolon character as separation.

   - **Bearer token:** If you select this method, you must fill the
       `Secret` field with the desired `user: password`. No
       whiteespaces, semicolon character as separation.

   - **Signature header:** If you select this method, you must fill
       the `Secret` field with the cryptographic key provided by the
       software on the other end.

   - **Certificate:** Select this option if you want to use a certificate uploaded via Sysdig's [Certificates Management](/en/certificates-management) tool.
       - The *Certificate* field will then appear; select the appropriate cert from the drop-down menu.

   **Secret:** Authorization / Authentication data. This field depends
      on the method selected in c).

   **Custom Headers** Any number of custom headers defined by the user to accommodate additional parameters required on the receiving end.

   To avoid interfering with the regular webhook protocol and expected headers, the [following headers](https://www.iana.org/assignments/message-headers/message-headers.xhtml) cannot be set using this form.

   **Data to Send:** Select from the drop-down the types of Sysdig data that should be forwarded. The available list depends on the Sysdig features and products you have enabled.

   Due to the heavy connection establishment overhead imposed by the  HTTP protocol, the Secure policy events are grouped by time proximity into batches and sent together in a single request as a  JSON array. In other words, every HTTP request will contain a JSON array containing one or more policy runtime events.

    Select whether or not you want to allow insecure connections (i.e.,  an invalid or self-signed certificate on the receiving side).

   Toggle the enable switch as necessary. Remember that you will need to "Test Integration" with the button below before enabling the integration.

5. Click **Save**.

## Configure Agent Local Forwarding

Review the [configuration steps](/en/event-forwarding/#configure-agent-local-forwarding) and use the following parameters for this integration.

| **Type** | **Attribute**   | **Required?** | **Type**             | **Allowed values**                               | **Default** | **Description**                                              |
| -------- | --------------- | ------------- | -------------------- | ------------------------------------------------ | ----------- | ------------------------------------------------------------ |
| WEBHOOK  | endpoint        | yes           | string               |                                                  |             | The webhook URL                                              |
| WEBHOOK  | secret          | no            | string               |                                                  |             | Secret to use, according to the “auth” value. Required if auth is set |
| WEBHOOK  | insecure        | no            | bool                 |                                                  | false       | Doesn’t verify TLS certificate                               |
| WEBHOOK  | auth            | no            | string               | BASIC_AUTH, BEARER_TOKEN, SIGNATURE              |             | Authentication methodology to use, optionally.               |
| WEBHOOK  | headers         | no            | sequence of mappings |                                                  |             | Extra headers to add to the request. Each header mapping requires 2 keys: “key” for the header key and “value” for its value |
| WEBHOOK  | output          | no            | string               | json, ndjson                                     | json        | Payload format                                               |
| WEBHOOK  | timestampFormat | no            | string               | seconds, milliseconds, microseconds, nanoseconds | nanoseconds | The resolution of the “timestamp” field in the payload       |
