---
linkTitle: "Forwarding to Syslog"
title: "Forwarding to Syslog"
weight: "37"
aliases:
  - /en/forwarding-to-syslog.html
  - /en/forward-syslog
description: '[Syslog](https://www.gnu.org/software/libc/manual/html_node/Overview-of-Syslog.html) refers to System Logging protocol. It is a standard chiefly used by network devices to send events and logs in a particular format to a centralized system for storage and analysis. A Syslog event includes severity level, host IP, timestamps, diagnostics information, and more.'
---

Sysdig Event Forwarding allows you to send events gathered by Sysdig Secure to a Syslog server.

## Prerequisites

Event forwards originate from region-specific IPs. For the full list of outbound IPs by region, see [SaaS Regions and IP Ranges](/en/docs/administration/saas-regions-and-ip-ranges/#saas-regions-and-ip-ranges).  Update your firewall and allow inbound requests from these IP addresses to enable Sysdig to handle event forwarding.

## Configure Standard Event Forwarding

To forward event data to a Syslog Server:

1. Log in to Sysdig Secure as Admin and go to **Profile > Settings > Event Forwarding**.

2. Click **+Add Integration** and choose **Syslog** from the drop-down menu.

3. Configure the required options:

   **Integration Name:** Define an integration name.

   **Address**: Specify the Syslog server where the events are
   forwarded. Enter a domain name or IP address. If a domain name
   resolves to several IP addresses, the first resolved address is
   used.

   **Port**: Specify the port number.

   **Protocol:** Choose the protocol depending on the server you are
   sending the logs to:

   * **RFC 3164**: RFC 3164 is the older version of the protocol, default
     port and transport is 514/UDP.

   * **RFC 5424**: RFC 5424 is the current version of the protocol,
     default port and transport is 514/UDP

   * **RFC 5425 (TLS):** RFC 5425 (TLS) is an extension to RFC 5424 to
     use an encrypted channel, default port and transport is 6514/TCP.  Select this option if you want to use a certificate uploaded via Sysdig's [Certificates Management](/en/certificates-management/) tool.

   **UDC/TCP:** Define transport layer protocol UDP/TCP. Use TCP for
   security incidents, as it's far more reliable than UDP for handling
   network congestion and preventing packet loss.

   * NOTE: RFC 5425 (TLS) only supports TCP.

   **Certificate:** (Optional) Select a certificate you've uploaded via Sysdig's [Certificates Management](/en/certificates-management/) tool. Note that the RFC 5425 (TLS) protocol is required for you to see this field.

   **Data to Send:** Select from the drop-down the types of Sysdig data that should be forwarded. The available list depends on the Sysdig features and products you have enabled.

   **Allow insecure connections:** Toggle on if you want to allow
   insecure connections (i.e. invalid or self-signed certificate on the
   receiving side).

   Toggle the enable switch as necessary. Remember that you will need
   to "Test Integration" with the button below before enabling the
   integration.

4. Click **Save**.

## Configure Agent Local Forwarding

Review the [configuration steps](/en/event-forwarding/#configure-agent-local-forwarding) and use the following parameters for this integration.

| **Type** | **Attribute** | **Required?** | **Type** | **Allowed values**           | **Default** | **Description**                                |
| -------- | ------------- | ------------- | -------- | ---------------------------- | ----------- | ---------------------------------------------- |
| SYSLOG   | ServicePort   | yes           | int      |                              |             | port of the syslog server                      |
| SYSLOG   | ServiceType   | no            | string   | tcp, udp                     | tcp         | protocol, tcp or udp (case insensitive)        |
| SYSLOG   | insecure      | no            | bool     |                              | true        | Doesn’t verify TLS certificate                 |
| SYSLOG   | MessageFormat | yes           | string   | RFC_3164, RFC_5424, RFC_5425 |             | The syslog message format. RFC5425 is TLS only |
| SYSLOG   | formatter     | no            | string   | JSON, LEEF, CEF              | JSON        | The message content format                     |
