---
title: "Uploading Custom CA Certificates"
linkTitle: "Upload Custom CA Certificates"
weight: "14"
aliases:
  - /en/docs/installation/serverless-agents/aws-fargate-serverless-agents/uploading-custom-ca-certificates/
  - /en/ecs-uploading-custom-cert
Description: "As of [serverless agent version 4.0.0](/en/docs/release-notes/serverless-agent-release-notes/#400-february-10-2023), the Orchestrator Agent supports the uploading the CA certificates."
---

## Upload CA Certificate Using CloudFormation

The CloudFormation template [orchestrator-agent.yaml >= 4.0.0](https://download.sysdig.com/dependencies/serverless/fargate/orchestrator-agent.yaml) contains a `Mappings` section that you can configure to provide the Orchestrator Agent with up two CA certificates for the OnPrem Collector and HTTP Proxy.

Note that the Orchestrator Agent can use both CA Certificates at the same time. 

```yaml
Mappings:
  # Upload custom CA certificates
  CACertificate:
    Collector:
      Type: "base64"
      Value: ""
      Path: "/ssl/collector_cert.pem"
    HttpProxy:
      Type: "base64"
      Value: ""
      Path: "/ssl/proxy_cert.pem"

  # Advanced configuration options
  Configuration:
    Collector:
      CACertificate: ""  # /ssl/collector_cert.pem
    HttpProxy:
      ...
      CACertificate: ""  # /ssl/proxy_cert.pem
```

#### OnPrem Collector

The table below describes the configuration parameters for setting up a CA certificate intended for the OnPrem Collector.

| Field                                   | Default                   | Description                                                                                                                                                                 |
|-----------------------------------------|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `CACertificate.Collector.Type`          | `base64`                  | The type of the certificate. Currently it supports `base64` only.                                                                                                           |
| `CACertificate.Collector.Value`         | ``                        | The certificate. Currently it supports `base64` encoded certificate only.                                                                                                   |
| `CACertificate.Collector.Path`          | `/ssl/collector_cert.pem` | The path (absolute, or relative to `/opt/draios`) to the CA Certificate in the Orchestrator Agent.                                                                          |
| `Configuration.Collector.CACertificate` | ``                        | The path to the CA certificate the Orchestrator Agent must use when connecting to the OnPrem Collector. It must be set to the same value as `CACertificate.Collector.Path`. |

#### HTTP Proxy

The table below describes the configuration parameters for setting up a custom CA certificate intended for the HTTP Proxy.

| Field                                   | Default               | Description                                                                                                                                                           |
|-----------------------------------------|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `CACertificate.HttpProxy.Type`          | `base64`              | The type of the certificate. Currently it supports `base64 only.                                                                                                      |
| `CACertificate.HttpProxy.Value`         | ``                    | The certificate. Currently it supports `base64` encoded certificate only.                                                                                             |
| `CACertificate.HttpProxy.Path`          | `/ssl/proxy_cert.pem` | The path (absolute, or relative to `/opt/draios) to the CA Certificate in the Orchestrator Agent.                                                                     |
| `Configuration.HttpProxy.CACertificate` | ``                    | The path to the CA certificate the Orchestrator Agent must use when connecting to the HTTP Proxy. It must be set to the same value as `CACertificate.HttpProxy.Path`. |


### Upload a CA Certificate for the OnPrem Collector

The example helps you configure the Orchestrator Agent to use a custom CA Certificate intended for a OnPrem Collector.

#### Encode your Certificate to `base64`

Encode the CA Certificate to `base64`.

There are several ways for doing so. For example, to encode the certificate `custom_ca.crt` in a Linux shell run: 

```shell
base64 custom_ca.crt
```

The result is a `base64` encoded string.

#### Configure the `orchestrator-agent.yaml` Template

Assuming the `base64` encoded CA Certificate for the OnPrem Collector is `myBase64EncodedCACertificate`, edit the [orchestrator-agent.yaml](https://download.sysdig.com/dependencies/serverless/fargate/orchestrator-agent.yaml) template as follows:
 - set `CACertificate.Collector.Value` to `myBase64EncodedCACertificate`, this provides the Orchestrator Agent with the CA Certificate. 
 - set `Configuration.Collector.CACertificate` to the same value as `CACertificate.Collector.Path`, this configures the Orchestrator Agent to use the uploaded certificate when connecting to the OnPrem Collector. 

```yaml
Mappings:
  # Upload custom CA certificates
  CACertificate:
    Collector:
      Type: "base64"
      Value: "myBase64EncodedCACertificate"
      Path: "/ssl/collector_cert.pem"
    ...

  # Advanced configuration options
  Configuration:
    Collector:
      CACertificate: "/ssl/collector_cert.pem"
    ...
```

#### Deploy the  `orchestrator-agent.yaml` Template

You can now deploy the template you just configured.

The Orchestrator Agent will:
 1. decode the `base64` encoded CA Certificate;
 2. store the decoded CA Certificate to the path defined in `CACertificate.Collector.Path`;
 3. use the CA Certificate defined in `Configuration.Collector.CACertificate` when connecting to the OnPrem Collector.


### Uploade a CA Certificate for an HTTP Proxy

The example helps you configure the Orchestrator Agent to use a custom CA Certificate intended for an HTTP Proxy.

Refer to [Enable HTTP Proxy for Serverless Agent](/en/install-ecs-fargate-secure/) for further details on how to configure the Orchestrator Agent to connect to an HTTP Proxy. 

#### Encode your Certificate to `base64`

Encode the CA Certificate to `base64`.

There are several ways for doing so. For example, to encode the certificate `custom_ca.crt` in a Linux shell run: 

```shell
base64 custom_ca.crt
```

The result is a `base64` encoded string.

#### Configure the `orchestrator-agent.yaml` Template

Assuming the `base64` encoded CA Certificate for the HTTP Proxy is `myBase64EncodedCACertificate`, edit the [orchestrator-agent.yaml](https://download.sysdig.com/dependencies/serverless/fargate/orchestrator-agent.yaml) template as follows:
 - set `CACertificate.HttpProxy.Value` to `myBase64EncodedCACertificate`, this provides the Orchestrator Agent with the CA Certificate. 
 - set `Configuration.HttpProxy.CACertificate` to the same value as `CACertificate.HttpProxy.Path`, this configures the Orchestrator Agent to use the uploaded certificate when connecting to the HTTP Proxy. 

```yaml
Mappings:
  # Upload custom CA certificates
  CACertificate:
    ...
    HttpProxy:
      Type: "base64"
      Value: "myBase64EncodedCACertificate"
      Path: "/ssl/proxy_cert.pem"

  # Advanced configuration options
  Configuration:
    ...
    HttpProxy:
      ...
      CACertificate: "/ssl/proxy_cert.pem"
```

#### Deploy the `orchestrator-agent.yaml` Template

You can now deploy the template you just configured.

The Orchestrator Agent will:
 1. decode the `base64` encoded CA Certificate;
 2. store the decoded CA Certificate to the path defined in `CACertificate.HttpProxy.Path`;
 3. use the CA Certificate defined in `Configuration.HttpProxy.CACertificate` when connecting to the HTTP Proxy.


## Upload CA Certificate Using Terraform

The Terraform module [fargate-orchestrator-agent >= 0.3.0](https://registry.terraform.io/modules/sysdiglabs/fargate-orchestrator-agent/aws/latest) exposes the following variables to provide the Orchestrator Agent with up two custom CA certificates for the OnPrem Collector and HTTP Proxy.

Note that the Orchestrator Agent can use both CA Certificates at the same time.

```hcl
variable "collector_ca_certificate" {
  description = "Uploads the collector custom CA certificate to the orchestrator"
  type = object({
    type  = string
    value = string
    path  = string
  })
  default = ({
    type  = "base64"
    value = ""
    path  = "/ssl/collector_cert.pem"
  })
}

variable "collector_configuration" {
  description = "Advanced configuration options for the connection to the collector"
  type = object({
    ca_certificate = string
  })
  default = ({
    ca_certificate = "" # /ssl/collector_cert.pem
  })
}

variable "http_proxy_ca_certificate" {
  description = "Uploads the HTTP proxy CA certificate to the orchestrator"
  type = object({
    type  = string
    value = string
    path  = string
  })
  default = ({
    type  = "base64"
    value = ""
    path  = "/ssl/proxy_cert.pem"
  })
}

variable "http_proxy_configuration" {
  description = "Advanced configuration options for the connection to the HTTP proxy"
  type = object({
    ...
    ca_certificate = string
  })
  default = ({
    ...
    ca_certificate  = "" # /ssl/proxy_cert.pem
  })
}
```

#### OnPrem Collector 

The table below describes the configuration parameters for setting up a CA certificate intended for the OnPrem Collector.

| Field                                    | Default                   | Description                                                                                                                                                                  |
|------------------------------------------|---------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `collector_ca_certificate.type`          | `base64`                  | The type of the certificate. Currently it supports `base64` only.                                                                                                            |
| `collector_ca_certificate.value`         | ``                        | The certificate. Currently it supports `base64` encoded certificate only.                                                                                                    |
| `collector_ca_certificate.path`          | `/ssl/collector_cert.pem` | The path (absolute, or relative to `/opt/draios`) to the CA Certificate in the Orchestrator Agent.                                                                           |
| `collector_configuration.ca_certificate` | ``                        | The path to the CA certificate the Orchestrator Agent must use when connecting to the OnPrem Collector. It must be set to the same value as `collector_ca_certificate.path`. |

#### HTTP Proxy

The table below describes the configuration parameters for setting up a custom CA certificate intended for the HTTP Proxy.

| Field                                     | Default               | Description                                                                                                                                                             |
|-------------------------------------------|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `http_proxy_ca_certificate.type`          | `base64`              | The type of the certificate. Currently it supports `base64 only.                                                                                                        |
| `http_proxy_ca_certificate.value`         | ``                    | The certificate. Currently it supports `base64` encoded certificate only.                                                                                               |
| `http_proxy_ca_certificate.path`          | `/ssl/proxy_cert.pem` | The path (absolute, or relative to `/opt/draios`) to the CA Certificate in the Orchestrator Agent.                                                                      |
| `http_proxy_configuration.ca_certificate` | ``                    | The path to the CA certificate the Orchestrator Agent must use when connecting to the HTTP Proxy. It must be set to the same value as `http_proxy_ca_certificate.path`. |


### Upload a CA Certificate for a OnPrem Collector

The example below describes how to configure the Orchestrator Agent to use a custom CA Certificate intended for a OnPrem Collector.

#### Encode your Certificate to `base64`

Encode the CA Certificate to `base64`.

There are several ways for doing so. For example, to encode the certificate `custom_ca.crt` in a Linux shell run: 

```shell
base64 custom_ca.crt
```

The result is a `base64` encoded string.

#### Configure the `fargate-orchestrator-agent` Module

Assuming the `base64` encoded CA Certificate for the OnPrem Collector is `myBase64EncodedCACertificate`, provides the module [fargate-orchestrator-agent >= 0.3.0](https://registry.terraform.io/modules/sysdiglabs/fargate-orchestrator-agent/aws/latest) with the variables that follow:
 - set `collector_ca_certificate.value` to `myBase64EncodedCACertificate`, this provides the Orchestrator Agent with the CA Certificate. 
 - set `collector_configuration.ca_certificate` to the same value as `collector_ca_certificate.path`, this configures the Orchestrator Agent to use the uploaded certificate when connecting to the OnPrem Collector. 

```hcl
module "fargate-orchestrator-agent" {
  source                    = "sysdiglabs/fargate-orchestrator-agent/aws"
  version                   = "0.3.1"

  vpc_id                    = "my_vpc_id"
  subnets                   = ["my_subnet_a", "my_subnet_b"]
  access_key                = "my-access-key"

  collector_host            = var.collector_host
  collector_port            = var.collector_port

  ...

  collector_ca_certificate = {
    type  = "base64"
    value = "myBase64EncodedCACertificate"
    path  = "/ssl/collector_cert.pem"
  }

  collector_ca_configuration  = {
    ca_certificate          = "/ssl/collector_cert.pem"
  }
}
```

#### Deploy the `fargate-orchestrator-agent` Module

You can now deploy the Terraform module.

The Orchestrator Agent will:
 1. decode the `base64` encoded CA Certificate;
 2. store the decoded CA Certificate to the path defined in `collector_ca_certificate.path`;
 3. use the CA Certificate defined in `collector_configuration.ca_certificate` when connecting to the OnPrem Collector.


### Upload a CA Certificate for an HTTP Proxy

The example below describes how to configure the Orchestrator Agent to use a custom CA Certificate intended for an HTTP Proxy.

Refer to [Enable HTTP Proxy for Serverless Agent](/en/install-ecs-fargate-secure/) for further details on how to configure the Orchestrator Agent to connect to an HTTP Proxy. 

#### Encode your certificate to `base64`

Encode the CA Certificate to `base64`.

There are several ways for doing so. For example, to encode the certificate `custom_ca.crt` in a Linux shell run: 

```shell
base64 custom_ca.crt
```

The result is a `base64` encoded string.

#### Configure the `fargate-orchestrator-agent` Module

Assuming the `base64` encoded CA Certificate for the HTTP Proxy is `myBase64EncodedCACertificate`, provides the module [fargate-orchestrator-agent >= 0.3.0](https://registry.terraform.io/modules/sysdiglabs/fargate-orchestrator-agent/aws/latest) with the variables that follow:
 - set `http_proxy_ca_certificate.value` to `myBase64EncodedCACertificate`, this provides the Orchestrator Agent with the CA Certificate. 
 - set `http_proxy_configuration.ca_certificate` to the same value as `http_proxy_ca_certificate.path`, this configures the Orchestrator Agent to use the uploaded certificate when connecting to the HTTP Proxy. 

```hcl
module "fargate-orchestrator-agent" {
  source           = "sysdiglabs/fargate-orchestrator-agent/aws"
  version          = "0.3.1"

  vpc_id           = "my_vpc_id"
  subnets          = ["my_subnet_a", "my_subnet_b"]

  access_key       = "my-access-key"

  ...

  http_proxy_ca_certificate = {
    type  = "base64"
    value = "myBase64EncodedCACertificate"
    path  = "/ssl/proxy_cert.pem"
  }

  http_proxy_configuration  = {
    ...
    ca_certificate          = "/ssl/proxy_cert.pem"
  }
}
```

#### Deploy the `fargate-orchestrator-agent` Module

You can now deploy the Terraform module.

The Orchestrator Agent will:
 1. Decode the `base64` encoded CA Certificate;
 2. Store the decoded CA Certificate to the path defined in `http_proxy_ca_certificate.path`;
 3. Use the CA Certificate defined in `http_proxy_configuration.ca_certificate` when connecting to the HTTP Proxy.
