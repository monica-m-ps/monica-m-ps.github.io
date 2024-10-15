---
linkTitle: "Forwarding to Splunk"
title: "Forwarding to Splunk"
weight: "29"
aliases:
  - /en/forwarding-to-splunk.html
  - /en/forward-splunk
description: "Splunk is a unified security and observability platform. Sysdig utilizes the HTTP Event Collector (HEC) to foward events to Splunk Enterprise and Splunk Cloud Platform."
---

## Prerequisites

Event forwards originate from region-specific IPs. For the full list of outbound IPs by region, see [SaaS Regions and IP Ranges](/en/docs/administration/saas-regions-and-ip-ranges/#saas-regions-and-ip-ranges). Update your firewall and allow inbound requests from these IP addresses to enable Sysdig to handle Splunk event forwarding.

Set up an [HEC](https://docs.splunk.com/Documentation/Splunk/latest/Data/UsetheHTTPEventCollector) on Splunk. Have the following on hand:

- The URL of the [HTTP Event Collector](https://docs.splunk.com/Documentation/Splunk/latest/Data/UsetheHTTPEventCollector) that forwards events to a Splunk deployment.
- The token generated when you create the Splunk Event Collector.

## Configure Standard Event Forwarding

To forward event data to Splunk:

1. Log in to Sysdig Secure as Admin and navigate to **Event Forwarding** via either **Integrations** or **Settings**.

2. Click **+Add Integration** and choose **HEC (Splunk)** from the drop-down menu.

3. Configure the required options:

   **Integration Name:** Define an integration name.

   **URL:** Define the URL of the Splunk service. This is the [HTTP Event Collector](https://docs.splunk.com/Documentation/Splunk/latest/Data/UsetheHTTPEventCollector) that forwards events to a Splunk deployment. Be sure to use the format `scheme://host:port`.

   **Token:** This is the token that Sysdig uses to authenticate the connection to the HTTP Event Collector. This token is created when you create the Splunk Event Collector.

   **Certificate:** If you have configured a [Certificates Management](/en/docs/administration/administration-settings/certificates-management/) tool, you can select one of your uploaded certs here.

   **Index**: The index where events are stored. Specify the Index if you have selected one while configuring the HTTP Event Collector.

   **Source Type**: Identifies the data structure of the event.
   For more information, see [Source Type](https://docs.splunk.com/Documentation/Splunk/7.3.1/Data/Whysourcetypesmatter).

   For more information on these parameters, see the
   [Splunk](https://docs.splunk.com/Splexicon) documentation.

   If left empty, each data type will have a source type. See [Appendix: Data Categories Mapped to Source Types](#appendix-data-categories-mapped-to-source-types) for details.

   **Data to Send:** Select from the drop-down the types of Sysdig data that should be forwarded. The available list depends on the Sysdig features and products you have enabled.

   Select whether or not you want to **Allow insecure connections**, such as invalid or self-signed certificate on the receiving side.

   Toggle the **Enabled** switch as necessary. You will need to **Test Integration** with the button below before enabling the integration.

4. Click  **Save**.

Here is an example of how policy events forwarded from Sysdig Secure are displayed on Splunk:

<img src="/image/384761885.png" height="250" />

## Configure Agent Local Forwarding

Review the [configuration steps](/en/event-forwarding/#configure-agent-local-forwarding) and use the following parameters for this integration.

| **Type** | **Attribute** | **Required?** | **Type** | **Allowed values** | **Default** | **Description**                                              |
| -------- | ------------- | ------------- | -------- | ------------------ | ----------- | ------------------------------------------------------------ |
| SPLUNK   | ServiceToken  | yes           | string   |                    |             | HTTP Event Collector Token                                   |
| SPLUNK   | ServiceURL    | yes           | string   |                    |             | URL of the Splunk instance                                   |
| SPLUNK   | Index         | no            | string   |                    |             | index to send data to. If unspecified, it will be used the index specified on the HTTP Event Collector configuration on Splunk. |
| SPLUNK   | ServiceType   | no            | string   | tcp, udp           | tcp         | protocol, tcp or udp (case insensitive)                      |
| SPLUNK   | insecure      | no            | bool     |                    | true        | Doesn’t verify TLS certificate                               |

## Reference: Data Categories Mapped to Source Types

| Sysdig Data Type | Splunk Source Type |
|------------------|--------------------|
| Monitor Events            | SysdigMonitor |
| Policy Events (Legacy)    | SysdigPolicy |
| Sysdig Platform Audit     | SysdigAuditTrail |
| Benchmark Events (Legacy) | SysdigSecureEvents |
| Secure events compliance (Legacy) | SysdigSecureEvents |
| Host Vulnerabilities (Legacy) | SysdigSecureEvents |
| Runtime Policy Events     | SysdigSecureEvents |
| Activity Audit            | SysdigActivityAudit |
