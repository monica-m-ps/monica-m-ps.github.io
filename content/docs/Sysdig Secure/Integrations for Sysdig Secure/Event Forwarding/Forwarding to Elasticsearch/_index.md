---
linkTitle: "Forwarding to Elasticsearch"
title: "Forwarding to Elasticsearch"
weight: "12"
aliases:
  - /en/forwarding-to-elasticsearch.html
  - /en/forwarding-to-elasticsearch
Description: "Elasticsearch is a distributed, RESTful search and analytics engine at the heart of the Elastic Stack. Sysdig provides event forwarding to Elasticsearch for versions greater or equal to Elasticsearch 7 and/or Opensearch 1.2. For more information, see [How to Ingest Data Into Elasticsearch Service](https://www.elastic.co/blog/how-to-ingest-data-into-elasticsearch-service)."
---

## Prerequisites

- Event forwards originate from region-specific IPs. For the full list of outbound IPs by region, see [SaaS Regions and IP Ranges](/en/docs/administration/saas-regions-and-ip-ranges/#saas-regions-and-ip-ranges).  Update your firewall and allow inbound requests from these IP addresses
to enable Sysdig to handle event forwarding.

- You must have an instance of Elasticsearch running and permissions to access it.

{{% callout type="note" %}}
  Datastreams are currently not supported. Make sure to configure your Elasticsearch index template with the "datastream" option set to off. That way, data will be stored on indices.
{{% /callout %}}

## Configure Standard Integration

1. Log in to Sysdig Secure as Admin and navigate to **Event Forwarding** via **Integrations** or **Settings**.

2. Click **+Add Integration** and choose **Elasticsearch** from the drop-down menu.

3. Configure the required options:

   **Integration Name:** Define an integration name.

    **Endpoint:** Enter the specific Elasticsearch instance where the data will be saved.  For ES Cloud and ES Cloud Enterprise, the endpoint can be found under the **Deployments** page:
     ![](/image/es_deploy.png)

    **Index Name:** Enter the name of the index under which the data will be stored. See [What is an Elasticsearch Index?](https://www.elastic.co/blog/what-is-an-elasticsearch-index) for details.

    **Authentication:**  Choose a method to authenticate API calls. Basic authentication uses the most common format of `username:password`. The given user must have `write` privileges in Elasticsearch; you can [query the available users](https://www.elastic.co/guide/en/elasticsearch/reference/current/users-command.html).

    **Secret**: If you selected Basic authentication: specify the `username:password` of a user with write privileges.

    **Labels and fields format**: Choose **Original format** or **Key value pairs**. See [Labels and Fields Format](#labels-and-fields-format)

    **Data to Send:** Select from the drop-down the types of Sysdig data that should be forwarded. The available list depends on the Sysdig features and products you have enabled.

    **Allow insecure connections:** Check the box if you wish to skip certificate validations when using HTTPS. You may wish to do this if you use self-signed ceriticates.

   Toggle the **Enabled** switch as necessary. You will need to **Test Integration** with the button below before enabling the integration.

5. Click **Save**.

### Labels and Fields Format

There are two formatting options for Elasticsearch event forwarding.

**Original format** sends events in the format shown in [Event Fowarding](/en/event-forwarding/#reference-json-formats-used-per-data-source). This format is not compatible with dynamic mapping.

**Key value pairs** sends events labels and `content.field` objects, such as `{"foo": "bar", ...}`, as arrays of objects: `[{"key": "foo", "value": "value"}, ...]`. This format is compatible with dynamic mapping.

**Key value pairs** is recommended for **Runtime Policy Events** or **Activity Audit**. If you prefer using **Original format** for these types of events, we recommend you disable dynamic mapping in Elasticsearch.

### Timestamp Mapping

To handle timestamps directly in Elasticsearch, you might want to map them to the appropriate field type. Timestamps have nanosecond resolution in Sysdig and they are available both in epoch timestamp and in RFC 3339 format.

The best approach is to use the [date_nanos](https://www.elastic.co/guide/en/elasticsearch/reference/current/date_nanos.html) field type and define an [explicit mapping](https://www.elastic.co/guide/en/elasticsearch/reference/current/explicit-mapping.html) in your Elasticsearch instance.

You will need to perform a `PUT /<index>/_mapping` API call with the index into which you are storing data, using the following payload:

```
{
 "properties": {
    "timestampRFC3339Nano": {
      "type": "date_nanos",
      "format": "strict_date_optional_time_nanos"
    }
  }
}
```

If you use the Kibana interface, you can do it there instead.

## Configure Agent Local Forwarding

Review the [configuration steps](/en/event-forwarding/#configure-agent-local-forwarding) and use the following parameters for this integration.

| **Type** | **Attribute** | **Required?** | **Type** | **Allowed values**      | **Default** | **Description**                                              |
| -------- | ------------- | ------------- | -------- | ----------------------- | ----------- | ------------------------------------------------------------ |
| ELASTIC  | endpoint      | yes           | string   |                         |             | Elasticsearch instance endpoint URL                          |
| ELASTIC  | indexName     | yes           | string   |                         |             | Name of the index to store the data in                       |
| ELASTIC  | insecure      | no            | bool     |                         | false       | Doesn’t verify TLS certificate                               |
| ELASTIC  | auth          | no            | string   | BASIC_AUTH,BEARER_TOKEN |             | Authentication method to use. Secret is required if this is specified |
| ELASTIC  | secret        | no            | string   |                         |             | Encoded basic authentication or bearer token value           |
