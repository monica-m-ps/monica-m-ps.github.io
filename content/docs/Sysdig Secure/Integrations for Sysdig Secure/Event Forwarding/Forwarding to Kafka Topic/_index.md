---
linkTitle: "Forwarding to Kafka Topic"
title: "Forwarding to Kafka Topic"
weight: "14"
aliases:
  - /en/forwarding-to-kafka-topic.html
  - /en/forward-kafka
  - /en/docs/sysdig-secure/secure-events/event-forwarding/forwarding-to-kafka-topic/
description: "Kafka is a distributed system consisting of servers and clients that communicate via a high-performance TCP network protocol. It can be deployed on bare-metal hardware, virtual machines, or containers on-premises as well as cloud environments. Sysdig event forwarding to Kafka is for Sysdig On-Prem users only."
---

Events are organized and durably stored in [topics](https://kafka.apache.org/intro). Very simplified, a topic is similar to a folder in a filesystem, and the events are the files in that folder.

## Prerequisites

This integration is only for **Sysdig On-Premises**.

## Configure Standard Integration

To forward secure data to Kafka:

1. Log in to Sysdig Secure On-Prem as Admin and go to **Profile > Settings > Event Forwarding**.

2. Click **+Add Integration** and choose **Kafka topic** from the drop-down menu.

3. Configure the required options:

    **Integration Name:** Define an integration name.

    **Brokers:** Kafka server endpoints. A Kafka cluster may provide
    several brokers; it follows the "hostname: port" (without protocol
    scheme). You can list several using a comma-separated list.

    **Topic:** Kafka topic where you want to store the forwarded data

    **Partitioner/Balancer**: Algorithm that the client uses to
    multiplex data between the multiple Brokers. For compatibility with
    the Java client, Murmur2 is used as the default partitioner.
    Supported algorithms are:

    - Murmur2

    - Round robin

    - Least bytes

    - Hash

    - CRC32

    **Compression:** Compression standard used for the data. Supported
    algorithms are:

    - LZ4

    - Snappy

    - Gzip

    - Standard

    **Data to Send:** Select from the drop-down the types of Sysdig data that should be forwarded. The available list depends on the Sysdig features and products you have enabled.

    Select whether or not you want to allow insecure connections (i.e.
    invalid or self-signed certificate on the receiving side).

    Toggle the enable switch as necessary. Remember that you will need
    to "Test Integration" with the button below before enabling the
    integration.

4. Click  **Save**.

## Configure Agent Local Forwarding

Review the [configuration steps](/en/event-forwarding/#configure-agent-local-forwarding) and use the following parameters for this integration.

| **Type** | **Attribute** | **Required?** | **Type** | **Allowed values**                           | **Default** | **Description**                                              |
| -------- | ------------- | ------------- | -------- | -------------------------------------------- | ----------- | ------------------------------------------------------------ |
| KAFKA    | brokers       | yes           | string   |                                              |             |                                                              |
| KAFKA    | topic         | yes           | string   |                                              |             |                                                              |
| KAFKA    | compression   | no            | string   | lz4, snappy, zstd, gzip                      |             |                                                              |
| KAFKA    | balancer      | no            | string   | roundrobin, leastbytes, hash, crc32, murmur2 | murmur2     |                                                              |
| KAFKA    | tls           | no            | bool     |                                              | false       |                                                              |
| KAFKA    | insecure      | no            | bool     |                                              | false       | Doesn’t verify TLS certificate                               |
| KAFKA    | auth          | no            | string   | gssapi                                       |             | The authentication method to optionally use. Currently supporting only GSSAPI |
| KAFKA    | principal     | no            | string   |                                              |             | GSSAPI principal. Required is GSSAPI authentication is selected |
| KAFKA    | realm         | no            | string   |                                              |             | GSSAPI realm. Required is GSSAPI authentication is selected  |
| KAFKA    | service       | no            | string   |                                              |             | GSSAPI Service name. Required is GSSAPI authentication is selected |
| KAFKA    | keytab        | no            | string   |                                              |             | base64 encoded Kerberos keytab for GSSAPI. Required is GSSAPI authentication is selected |
| KAFKA    | krb5          | no            | string   |                                              |             | Kerberos krb5.conf file content for GSSAPI. Required is GSSAPI authentication is selected |
