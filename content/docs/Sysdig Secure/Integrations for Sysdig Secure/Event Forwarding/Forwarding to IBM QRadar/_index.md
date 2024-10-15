---
linkTitle: "Forwarding to IBM QRadar"
title: "Forwarding to IBM QRadar"
weight: "20"
aliases:
  - /en/forwarding-to-ibm-qradar.html
  - /en/forward-qradar
Description: "IBM Security® QRadar® Suite is a modernized threat detection and response solution designed to unify the security analyst experience and accelerate their speed across the full incident lifecycle."
---

## Prerequisites

Event forwards originate from region-specific IPs. For the full list of outbound IPs by region, see [SaaS Regions and IP Ranges](/en/docs/administration/saas-regions-and-ip-ranges/#saas-regions-and-ip-ranges).  Update your firewall and allow inbound requests from these IP addresses to enable Sysdig to handle event forwarding.

## Configure Standard Integration

To forward event data to [IBM QRadar](https://www.ibm.com/qradar):

1. Log in to Sysdig Secure as Admin and go to **Profile > Settings > Event Forwarding**.

2. Click **+Add Integration** and choose **IBM QRadar** from the drop-down menu.

   {{% callout type="warning" %}}
   Although QRadar receives Syslog messages, it uses [LEEF format](https://www.ibm.com/docs/en/dsm?topic=overview-leef-event-components).
   Using Syslog as the source type will potentially generate incompatible messages with IBM QRadar.
   {{% /callout %}}

3. Configure the required options:

   **Integration Name:** Define an integration name.

   **Address**: Specify the DNS address of the QRadar installation
   endpoint.

   **Port**: TCP Port to send data. 514 is the default

   **Data to Send:** Select from the drop-down the types of Sysdig data that should be forwarded. The available list depends on the Sysdig features and products you have enabled.

   **Allow insecure connections:** Toggle on if you want to allow insecure connections (i.e. invalid or self-signed certificate on the receiving side).

   Toggle the enable switch as necessary. Remember that you will need to "Test Integration" with the button below before enabling the integration.

4. Click **Save**.

## Configure Agent Local Forwarding

Review the [configuration steps](/en/event-forwarding/#configure-agent-local-forwarding) and use the following parameters for this integration.

| **Type** | **Attribute** | **Required?** | **Type** | **Allowed values** | **Default** | **Description**                                              |
| -------- | ------------- | ------------- | -------- | ------------------ | ----------- | ------------------------------------------------------------ |
| QRADAR   | address       | yes           | string   |                    |             | DNS name or IP of the QRadar instance                        |
| QRADAR   | port          | yes           | int      |                    |             | Port exposed to receive Syslog events. 6514 or 514 by default. |
| QRADAR   | insecure      | no            | bool     |                    | false       | Doesn’t verify TLS certificate                               |
| QRADAR   | tls           | no            | bool     |                    | false       |                                                              |

## Learn More

See [IBM Documentation](https://www.ibm.com/docs/en/dsm?topic=management-adding-log-source) for information on log sources.
