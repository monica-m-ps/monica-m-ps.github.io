---
linkTitle: "Cluster Shield"
title: "Configuration Library for Cluster Shield"
weight: "14"
aliases:
  - /en/configuration-library-cluster-shield
  - /en/configure-cluster-shield
  - /en/docs/installation/configuration/cluster-shield/
Description: "The Cluster Shield configuration library lists all the major configurations supported by Cluster Shield components. This document is evolving and will be updated as new configurations are added to the product."
---

## Generic Configuration

| Property        | Description      | Required | Default |
| --------------- | ------------------------------------------------------------------------------------------------------------------------ | -------- | ------- |
| cache           | Configuration for the cluster shield cache.                          | No       |         |
| cluster_config  | The name of the cluster. Set a unique value for all the clusters being inspected.              | Yes      |         |
| features        | Features configurations.                   | Yes      |         |
| kubernetes      | Kubernetes configurations.                 | Yes      |         |
| log_level       | The minimum log severity to be reported in logs. Expected one of the following: `err` ,`warn` ,`info` ,`debug` ,`trace`. | Yes      | `warn`  |
| monitoring_port | The HTTP Server port used to expose healthcheck and prometheus metrics.                        | No       | `8080`  |
| ssl             | SSL configurations.                        | Yes      |         |
| sysdig_endpoint | The configuration for the sysdig services.                           | Yes      |         |

## Features

| Property | Description    | Type                  | Required | Default | Example |
| ---------------------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------- | -------- | ------- | ------- |
| admission_control                  | Configurations for the admission control feature. This feature is in active development. See [Admission Control](#admission-control). | [Admission Control](#admission-control)         | Yes      |         |         |
| audit    | Configurations for the audit feature.    | [Audit](#audit)       | Yes      |         |         |
| container_vulnerability_management | Configurations for the container vulnerability management feature. | [Container Vulnerability Management](#container-Vulnerability-management) | Yes      |         |         |
| kubernetes_metadata                | Configurations for the Kubernetes metadata feature.                | [Kubernetes Metadata](#kubernetes-metadata)     | Yes      |         |         |
| posture  | Configurations for the posture feature.  | [Posture](#posture)   | Yes      |         |         |

## Kubernetes

| Property             | Description                  | Type   | Required | Default       | Example         |
| -------------------- | ------------------------------------------------------ | ------ | -------- | ------------- | --------------- |
| ca_cert_file         | Path to the CA Certificate file.                       | string | No       |               | `/cert/ca.crt`  |
| root_namespace       | Root namespace to use for the kubernetes resources.    | string | No       | `kube-system` | `kube-system`   |
| running_namespace    | Current namespace to use for the kubernetes resources. | string | No       |               | `sysdig-agent`  |
| tls_cert_file        | Path to the TLS Certificate file.                      | string | No       |               | `/cert/tls.crt` |
| tls_private_key_file | Path to the TLS Private Key file.                      | string | No       |               | `/cert/tls.key` |

## SSL

| Property | Description                         | Type    | Required | Default | Example |
| -------- | ------------------------------------------------------------- | ------- | -------- | ------- | ------- |
| verify   | Define if the client must verify the backend SSL certificate. | boolean | Yes      | `true`  |         |

## Sysdig Endpoint

| Property         | Description                                   | Type   | Required | Default  | Example      |
|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------|----------|----------|----------------------------------------|
| access_key       | Sysdig Agent Access Key.   | string | Yes      |          | `12345678-1234-1234-1234-123456789012` |
| api_url          | Sysdig backend host. Expected format: `uri`.     | string | Yes      |          | `https://www.example.com`              |
| collector        | Host and port to access Sysdig Collector endpoint. Expected format: `hostport`.                                             | string | No       |          | `collector.example.com:6443`           |
| region           | The region where the collector is located. Expected one of: `custom` ,`au-syd-monitor` ,`au-syd-private-monitor` ,`au-syd-private-secure` ,`au-syd-secure` ,`au1` ,`br-sao-monitor` ,`br-sao-private-monitor` ,`br-sao-private-secure` ,`br-sao-secure` ,`ca-tor-monitor` ,`ca-tor-private-monitor` ,`ca-tor-private-secure` ,`ca-tor-secure` ,`eu-de-monitor` ,`eu-de-private-monitor` ,`eu-de-private-secure` ,`eu-de-secure` ,`eu-gb-monitor` ,`eu-gb-private-monitor` ,`eu-gb-private-secure` ,`eu-gb-secure` ,`eu1` ,`in1` ,`jp-osa-monitor` ,`jp-osa-private-monitor` ,`jp-osa-private-secure` ,`jp-osa-secure` ,`jp-tok-monitor` ,`jp-tok-private-monitor` ,`jp-tok-private-secure` ,`jp-tok-secure` ,`me2` ,`us-east-monitor` ,`us-east-private-monitor` ,`us-east-private-secure` ,`us-east-secure` ,`us-south-monitor` ,`us-south-private-monitor` ,`us-south-private-secure` ,`us-south-secure` ,`us1` ,`us2` ,`us3` ,`us4`. | string | Yes      | `custom` |              |
| secure_api_token | The API Token to access Sysdig Secure.                                  | string | No       |          | `12345678-1234-1234-1234-123456789012` |

## Admission Control

{{% callout type="note" %}}
The new Admission Control feature for Vulnerability Management (VM) and Kubernetes Security Posture Management (KSPM) in Cluster Shield is in active development. If you are interested in using this feature, contact your Sysdig representative. 
{{% /callout %}}

| Property | Description    | Type                    | Required | Default | Example |
|------------------------------------|--------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|----------|---------|---------|
| deny_on_error                      | Deny request when an error happens inside the evaluation phase.    | boolean                 | Yes      | `false` |         |
| dry_run  | Dry Run requests.                        | boolean                 | No       | `true`  |         |
| enabled  | Specify if the Admission Control is enabled.                       | boolean                 | Yes      | `false` |         |
| excluded_namespaces                | List of namespaces to exclude from the admission control feature.  | array[string]           | No       | `[]`    |         |
| http_port                          | The HTTP Server port to expose the webhook web server.             | integer                 | Yes      | `8443`  |         |
| timeout  | The number of seconds for the request to time out.                 | integer                 | No       | `5`     |         |
| container_vulnerability_management | Configurations for the container vulnerability management feature. | [AdmissionControlContainerVulnerabilityManagement](#AdmissionControlContainerVulnerabilityManagement) | Yes      |         |         |

## AdmissionControlContainerVulnerabilityManagement

| Property | Description             | Type    | Required | Default | Example |
| -------- | ------------------------------------------------- | ------- | -------- | ------- | ------- |
| enabled  | Enable container vulnerability management checks. | boolean | No       | `false`  |         |

## Audit

| Property            | Description                   | Type          | Required | Default | Example |
|---------------------|---------------------------------------------------------|---------------|----------|---------|---------|
| enabled             | Specify if the audit feature is enabled.                | boolean       | Yes      | `false` |         |
| excluded_namespaces | List of namespaces to exclude from the audit feature.   | array[string] | No       | `[]`    |         |
| http_port           | HTTP Server port used to expose the webhook web server. | integer       | Yes      | `6443`  |         |
| timeout             | The number of seconds for the request to time out.      | integer       | Yes      | `5`     |         |

## Cluster Configuration

| Property | Description               | Type   | Required | Default | Example      |
|----------|-----------------------------------------------------------------------------------------------------------------------------------|--------|----------|---------|--------------|
| name     | The name of the cluster. Make sure to set a unique value for all the clusters being inspected.          | string | Yes      |         | `my-cluster` |
| tags     | Tags you want to apply to the metadata sent to the Sysdig BE. They are used for instance as additional labels to the KSM metrics. | object | No       |         |              |

## Container Vulnerability Management

| Property                      | Description                         | Type                        | Required | Default     | Example |
|-------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|----------|-------------|---------|
| enabled                       | Specify if the scanning feature is enabled.                   | boolean                     | Yes      | `false`     |         |
| in_use                        |           | [ContainerVulnerabilityManagementInUse](#containervulnerabilitymanagementinuse) | Yes      |             |         |
| local_cluster                 |           | [ContainerVulnerabilityManagementLocal](#containervulnerabilitymanagementlocal) | Yes      |             |         |
| max_file_size_bytes           | The maximum size in bytes allowed for a file to be analyzed.              | integer                     | No       | `104857600` |         |
| max_file_size_bytes_in_memory | The maximum size in bytes for a file to be analyzed in memory. The files larger than this size are temporarily copied to the filesystem. | integer                     | No       | `26214400`  |         |
| parallel_files_analysis_count | The maximum number of files that are analyzed in parallel.        | integer                     | No       | `5`         |         |
| platform_services_enabled     | Specify if the platform services are enabled.                 | boolean                     | No       | `true`      |         |
| registry_ssl                  | Verify SSL certificate when connecting to the registry.       | [SSL](#SSL)                 | Yes      |             |         |

## ContainerVulnerabilityManagementInUse

| Property            | Description                      | Type    | Required | Default | Example |
| ------------------- | ------------------------------------------------------------------------------------ | ------- | -------- | ------- | ------- |
| enabled             | Retrieve in-use information from the backend and aggregate them on the scan results. | boolean | Yes      | `true`  |         |
| integration_enabled | Share in-use information with the external integrations.   | boolean | Yes      | `false` |         |

## ContainerVulnerabilityManagementLocal

| Property         | Description          | Type                          | Required | Default | Example |
| ---------------- | ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------- | -------- | ------- | ------- |
| registry_secrets |                      | [ContainerVulnerabilityManagementLocalRegistrySecret](#containervulnerabilitymanagementlocalregistrysecret) | No       |         |         |

## ContainerVulnerabilityManagementLocalRegistrySecret

| Property  | Description | Type          | Required | Default | Example |
| --------- | ----------- | ------------- | -------- | ------- | ------- |
| namespace |             | string        | Yes      |         |         |
| secrets   |             | array[string] | Yes      |         |         |

## Kubernetes Metadata

| Property              | Description               | Type    | Required | Default | Example |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|----------|---------|---------|
| enabled               | Specify if the Kubernetes Metadata feature is enabled.                        | boolean | Yes      | `false` |         |
| annotations_allowlist | List of Kubernetes annotations keys that will be sent to the Sysdig BE e.g. for generating KSM metrics. To include them, provide a list of resource names in their plural form and Kubernetes annotation keys you would like to allow for them. Annotation keys can contain wildcard character '*'. A single '*' can be provided per resource to allow any annotation. By default, no annotations are allowed for any resource. | object  | No       | `{}`    |         |

## Posture

| Property | Description      | Type    | Required | Default | Example |
| -------- | ------------------------------------------ | ------- | -------- | ------- | ------- |
| enabled  | Specify if the Posture feature is enabled. | boolean | Yes      | `false` |         |

## Cache

| Property  | Description                    | Type                        | Required | Default | Example |
| --------- | -------------------------------------------------------- | --------------------------- | -------- | ------- | ------- |
| backend | Define the cache backend to use. Expected one of: `redis`. | `string`                    | No       |         |         |
| redis   | Configuration for the cluster shield redis cache.          | [CacheRedis](#CacheRedis)   | No       |         |         |

## CacheRedis

| Property           | Description | Type    | Required | Default | Example |
| ------------------ | ----------- | ------- | -------- | ------- | ------- |
| address            |             | string  | No       |         |         |
| database           |             | string  | No       |         |         |
| password           |             | string  | No       |         |         |
| prefix             |             | string  | No       |         |         |
| sentinel_addresses |             | string  | No       |         |         |
| sentinel_master    |             | string  | No       |         |         |
| tls_ca             |             | string  | No       |         |         |
| tls_enabled        |             | boolean | No       |         |         |
| tls_skip           |             | boolean | No       |         |         |
| ttl                |             | string  | No       |         |         |
| username           |             | string  | No       |         |         |
