---
linkTitle: "Enable HTTP Proxy for Serverless Agents"
title: "Enable HTTP Proxy for Serverless Agents"
weight: "11"
aliases:
  - /en/docs/installation/serverless-agents/aws-fargate-serverless-agents/enable-http-proxy-for-serverless-agents/
  - /en/enable-ecs-http-proxy
  - /en/docs/sysdig-secure/install-agent-components/configuration/ecs-fargate-serverless-agent/enable-http-proxy-for-serverless-agents/
Description: "Both the orchestrator and the Workload Agent can be configured to connect to an HTTP proxy."
---

## Orchestrator Agent

### CloudFormation

As of [serverless agent version 4.0.0](/en/docs/release-notes/serverless-agent-release-notes/#400-february-10-2023),  the CloudFormation template [orchestrator-agent.yaml](https://download.sysdig.com/dependencies/serverless/fargate/orchestrator-agent.yaml) contains a `Mapping` section to configure the orchestrator to connect to an HTTP proxy.

```yaml
Mappings:
  # Upload custom CA certificates
  CACertificate:
    ...
    HttpProxy:
      Type: "base64"
      Value: ""
      Path: "/ssl/proxy_cert.pem"
    ...

  # Advanced configuration options
  Configuration:
    ...
    HttpProxy:
      ProxyHost: ""
      ProxyPort: ""
      ProxyUser: ""
      ProxyPassword: ""  # Cleartext or SecretsManager secret reference (arn:aws:secretsmanager:region:aws_account_id:secret:secret-name:json-key:version-stage:version-id)"
      SSL: ""
      SSLVerifyCertificate: ""
      CACertificate: ""  # /ssl/proxy_cert.pem
```

The section:
 - `CACertificate.HttpProxy` supports the uploading of a CA Certificate for the HTTP proxy, as described in [Uploading Custom CA Certificates](/en/install-ecs-fargate-secure/).
 - `Configuration.HttpProxy` configures the orchestrator to connect to the HTTP proxy.

The following table describes each configuration option in detail. 

| Field                | Default | Description                |
|------------------------------------------------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `Configuration.HttpProxy.ProxyHost`            | ``      | Indicates the hostname of the proxy server. The default is an empty string, which implies communication through an HTTP proxy is disabled. \\ Maps to `http_proxy.proxy_host` in [Enable HTTP proxy for agents](/en/enable-http-proxy-for-agents).        |
| `Configuration.HttpProxy.ProxyPort`            | ``      | Specifies the port on the proxy server the agent should connect to. The default is 0, which indicates that the HTTP proxy is disabled. \\ Maps to `http_proxy.proxy_port` in [Enable HTTP proxy for agents](/en/enable-http-proxy-for-agents).            |
| `Configuration.HttpProxy.ProxyUser`            | ``      | Required if HTTP authentication is configured. This option specifies the username for the HTTP authentication. The default is an empty string, which indicates that authentication is not configured. \\ Maps to `http_proxy.proxy_user` in [Enable HTTP proxy for agents](/en/enable-http-proxy-for-agents).                           |
| `Configuration.HttpProxy.ProxyPassword`        | ``      | Supports both cleartext password and SecretsManager-backed passwords. Required if HTTP authentication is configured. This option specifies the password for the HTTP authentication. The default is an empty string. Specifying `proxy_user` with no proxy_password is allowed. \\ Maps to `http_proxy.proxy_password` in [Enable HTTP proxy for agents](/en/enable-http-proxy-for-agents). |
| `Configuration.HttpProxy.SSL`                  | ``      | Defaults to `false` if not provided. If set to `true`, the connection between the agent and the proxy server is encrypted. \\ Maps to `http_proxy.ssl` in [Enable HTTP proxy for agents](/en/enable-http-proxy-for-agents).                               |
| `Configuration.HttpProxy.SSLVerifyCertificate` | ``      | Defaults to `true` if not provided. Determines whether the agent will verify the certificate presented by the proxy. \\ Maps to `http_proxy.ssl_verify_certificate` in [Enable HTTP proxy for agents](/en/enable-http-proxy-for-agents).                  |
| `Configuration.HttpProxy.CACertificate`        | ``      | The path to the certificate in the Orchestrator. Use the same value as `CACertificate.HttpProxy.Path` to use the uploaded certificate. \\ Maps to `http_proxy.ca_certificate` in [Enable HTTP proxy for agents](/en/enable-http-proxy-for-agents).        |


#### Examples

##### HTTP proxy with user and cleartext password

The following configuration shows how to configure the `orchestrator-agent.yaml` template to enable the Orchestrator Agent to connect to the HTTP proxy with:
 - `squid.my.domain.com:6444` as a host and port;
 - `my-user` as a username;
 - `my-proxy-password` as a cleartext password.

```yaml
Mappings:
  # Advanced configuration options
  Configuration:
    ...
    HttpProxy:
      ProxyHost: "squid.my.domain.com"
      ProxyPort: "6443"
      ProxyUser: "my-user"
      ProxyPassword: "my-proxy-password"
      SSL: ""
      SSLVerifyCertificate: ""
      CACertificate: ""
```

##### HTTP proxy with user and AWS SecretsManager-backed password

The HTTP proxy password can be fetched from AWS SecretsManager as well.
The following configuration shows how to configure the `orchestrator-agent.yaml` template to enable the Orchestrator Agent to connect to the HTTP proxy with:
 - `squid.my.domain.com:6444` as a host and port;
 - `my-user` as a username;
 - `arn:aws:secretsmanager:us-east-1:############:secret:json-secret-ffKpFB:SysdigAccessKey::` as a password to be fetched from a JSON secret.

Refer to [Fetching Secrets from SercretsManager](/en/install-ecs-fargate-secure/) for further details and examples on how to fetch secrets from AWS SecretsManager.

```yaml
Mappings:
  # Advanced configuration options
  Configuration:
    ...
    HttpProxy:
      ProxyHost: "squid.my.domain.com"
      ProxyPort: "6443"
      ProxyUser: "my-user"
      ProxyPassword: "arn:aws:secretsmanager:us-east-1:############:secret:json-secret-ffKpFB:SysdigAccessKey::"
      SSL: ""
      SSLVerifyCertificate: ""
      CACertificate: ""
```


##### HTTP proxy with CA certificate

Also, you can provide a CA Certificate to the Orchestrator Agent and configure it to check the HTTP proxy SSL Certificate.

Refer to [Uploading Custom CA Certificates](/en/install-ecs-fargate-secure/) for further details and examples on how to upload and use CA Certificates.

```yaml
Mappings:
  # Upload custom CA certificates
  CACertificate:
    ...
    HttpProxy:
      Type: "base64"
      Value: "my-base64-encoded-ca-certificate"
      Path: "/ssl/proxy_cert.pem"
    ...

  # Advanced configuration options
  Configuration:
    ...
    HttpProxy:
      ProxyHost: "squid.my.domain.com"
      ProxyPort: "6443"
      ProxyUser: "my-user"
      ProxyPassword: "arn:aws:secretsmanager:us-east-1:############:secret:json-secret-ffKpFB:SysdigAccessKey::"
      SSL: "true"
      SSLVerifyCertificate: "true"
      CACertificate: "/ssl/proxy_cert.pem"
```


### Terraform

The Terraform module [fargate-orchestrator-agent >= 0.3.0](https://registry.terraform.io/modules/sysdiglabs/fargate-orchestrator-agent/aws/latest) exposes the following variables to configure the Orchestrator Agent to connect to an HTTP proxy.

```hcl
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
    proxy_host             = string
    proxy_port             = string
    proxy_user             = string
    proxy_password         = string
    ssl                    = string
    ssl_verify_certificate = string
    ca_certificate         = string
  })
  default = ({
    proxy_host             = ""
    proxy_port             = ""
    proxy_user             = ""
    proxy_password         = ""
    ssl                    = ""
    ssl_verify_certificate = ""
    ca_certificate         = "" # /ssl/proxy_cert.pem
  })
}
```

The object:
 - `http_proxy_ca_certificate` supports the uploading of a CA Certificate for the HTTP proxy, as described in [Uploading Custom CA Certificates](/en/install-ecs-fargate-secure/).
 - `http_proxy_configuration` configures the orchestrator to connect to the HTTP proxy.

The following table describes each configuration option in detail. 

| Field                   | Default | Description                |
|---------------------------------------------------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `http_proxy_configuration.proxy_host`             | ``      | Indicates the hostname of the proxy server. The default is an empty string, which implies communication through an HTTP proxy is disabled. \\ Maps to `http_proxy.proxy_host` in [Enable HTTP proxy for agents](/en/enable-http-proxy-for-agents).        |
| `http_proxy_configuration.proxy_port`             | ``      | Specifies the port on the proxy server the agent should connect to. The default is 0, which indicates that the HTTP proxy is disabled. \\ Maps to `http_proxy.proxy_port` in [Enable HTTP proxy for agents](/en/enable-http-proxy-for-agents).            |
| `http_proxy_configuration.proxy_user`             | ``      | Required if HTTP authentication is configured. This option specifies the username for the HTTP authentication. The default is an empty string, which indicates that authentication is not configured. \\ Maps to `http_proxy.proxy_user` in [Enable HTTP proxy for agents](/en/enable-http-proxy-for-agents).                           |
| `http_proxy_configuration.proxy_password`         | ``      | Supports both cleartext password and SecretsManager-backed passwords. Required if HTTP authentication is configured. This option specifies the password for the HTTP authentication. The default is an empty string. Specifying `proxy_user` with no proxy_password is allowed. \\ Maps to `http_proxy.proxy_password` in [Enable HTTP proxy for agents](/en/enable-http-proxy-for-agents). |
| `http_proxy_configuration.ssl`                    | ``      | Defaults to `false` if not provided. If set to `true`, the connection between the agent and the proxy server is encrypted. \\ Maps to `http_proxy.ssl` in [Enable HTTP proxy for agents](/en/enable-http-proxy-for-agents).                               |
| `http_proxy_configuration.ssl_verify_certificate` | ``      | Defaults to `true` if not provided. Determines whether the agent will verify the certificate presented by the proxy. This option is enabled by default. \\ Maps to `http_proxy.ssl_verify_certificate` in [Enable HTTP proxy for agents](/en/enable-http-proxy-for-agents).         |
| `http_proxy_configuration.ca_certificate`         | ``      | The path (relative to /opt/draios) to the certificate in the Orchestrator.Use the same value as CACertificate.HttpProxy.Path to use the uploaded certificate. \\ Maps to `http_proxy.ca_certificate` in [Enable HTTP proxy for agents](/en/enable-http-proxy-for-agents).           |

##### HTTP proxy with user and cleartext password

The following configuration shows how to configure the `orchestrator-agent.yaml` template to enable the Orchestrator Agent to connect to the HTTP proxy with:
 - `squid.my.domain.com:6444` as a host and port;
 - `my-user` as a username;
 - `my-proxy-password` as a cleartext password.

```yaml
module "fargate-orchestrator-agent" {
  source                   = "sysdiglabs/fargate-orchestrator-agent/aws"
  version                  = "0.3.1"

  vpc_id                   = "my_vpc_id"
  subnets                  = ["my_subnet_a", "my_subnet_b"]

  access_key               = "my-access-key"

  ...

  http_proxy_configuration = {
    proxy_host             = "squid.my.domain.com"
    proxy_port             = "6443"
    proxy_user             = "my-user"
    proxy_password         = "my-proxy-password"
    ssl                    = ""
    ssl_verify_certificate = ""
    ca_certificate         = ""
  }
}
```

##### HTTP proxy with user and AWS SecretsManager-backed password

The HTTP proxy password can be fetched from AWS SecretsManager as well.
The following configuration shows how to configure the `orchestrator-agent.yaml` template to enable the Orchestrator Agent to connect to the HTTP proxy with:
 - `squid.my.domain.com:6444` as a host and port;
 - `my-user` as a username;
 - `arn:aws:secretsmanager:us-east-1:############:secret:json-secret-ffKpFB:SysdigAccessKey::` as a password to be fetched from a JSON secret.

Refer to [Fetching Secrets from SercretsManager](/en/install-ecs-fargate-secure/) for further details and examples on how to fetch secrets from AWS SecretsManager.

```yaml
module "fargate-orchestrator-agent" {
  source                   = "sysdiglabs/fargate-orchestrator-agent/aws"
  version                  = "0.3.1"

  vpc_id                   = "my_vpc_id"
  subnets                  = ["my_subnet_a", "my_subnet_b"]

  access_key               = "my-access-key"

  ...

  http_proxy_configuration = {
    proxy_host             = "squid.my.domain.com"
    proxy_port             = "6443"
    proxy_user             = "my-user"
    proxy_password         = "arn:aws:secretsmanager:us-east-1:############:secret:json-secret-ffKpFB:SysdigAccessKey::"
    ssl                    = ""
    ssl_verify_certificate = ""
    ca_certificate         = ""
  }
}
```

##### HTTP proxy with CA certificate

Also, you can provide a CA Certificate to the Orchestrator Agent and configure it to check the HTTP proxy SSL Certificate.

Refer to [Uploading Custom CA Certificates](/en/install-ecs-fargate-secure/) for further details and examples on how to upload and use CA Certificates.

```yaml
module "fargate-orchestrator-agent" {
  source                    = "sysdiglabs/fargate-orchestrator-agent/aws"
  version                   = "0.3.1"

  vpc_id                    = "my_vpc_id"
  subnets                   = ["my_subnet_a", "my_subnet_b"]

  access_key                = "my-access-key"

  ...

  http_proxy_ca_certificate = {
    type  = "base64"
    value = "my-base64-encoded-ca-certificate"
    path  = "/ssl/proxy_cert.pem"
  }

  http_proxy_configuration  = {
    proxy_host              = "squid.my.domain.com"
    proxy_port              = "6443"
    proxy_user              = "my-user"
    proxy_password          = "arn:aws:secretsmanager:us-east-1:############:secret:json-secret-ffKpFB:SysdigAccessKey::"
    ssl                     = "true"
    ssl_verify_certificate  = "true"
    ca_certificate          = "/ssl/proxy_cert.pem"
  }
}
```


### Container Environment Variable

Alternatively, you can provide the environment variable `ADDITIONAL_CONF` to the container running the Orchestrator Agent to configure it to connect to an HTTP proxy. 

The following configuration options affect the behavior of the HTTP proxy setting, and can be specified either in JSON or YAML format under `http_proxy` heading.
Refer to [Enable HTTP proxy for agents](/en/enable-http-proxy-for-agents) for further details.

| Option                   | Default | Description           |
|--------------------------|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `proxy_host`             | ``      | Indicates the hostname of the proxy server. The default is an empty string, which implies communication through an HTTP proxy is disabled.              |
| `proxy_port`             | ``      | Specifies the port on the proxy server the agent should connect to. The default is 0, which indicates that the HTTP proxy is disabled.                  |
| `proxy_user`             | ``      | Required if HTTP authentication is configured. This option specifies the username for the HTTP authentication. The default is an empty string, which indicates that authentication is not configured.       |
| `proxy_password`         | ``      | Required if HTTP authentication is configured. This option specifies the password for the HTTP authentication. The default is an empty string. Specifying `proxy_user` with no `proxy_password` is allowed. |
| `ssl`                    | `false` | Default: `false`. If set to `true`, the connection between the agent and the proxy server is encrypted.                       |
| `ssl_verify_certificate` | `true`  | Determines whether the agent will verify the certificate presented by the proxy.                    |
| `ca_certificate`         | `true`  | The path to the CA certificate for the proxy server. If `ssl_verify_certificate` is enabled, the CA certificate must be signed appropriately.           |


For example, the following configuration shows how to configure the Orchestrator Agent to connect to the HTTP proxy with:
 - `squid.my.domain.com:6444` as a host and port;
 - `my-user` as a username;
 - `my-proxy-password` as a password.

```yaml
http_proxy:
  proxy_host: squid.my.domain.com
  proxy_port: 6443
  proxy_user: my-user
  proxy_password: my-proxy-password
```

Note that newlines and spaces matter when passing such a configuration as a YAML to the Workload Agent.

```shell
ADDITIONAL_CONF="http_proxy:\n  proxy_host: squid.my.domain.com\n  proxy_port: 6443\n  proxy_user: my-user\n  proxy_password: my-proxy-password"
```

This option supports cleartext passwords only.


## Workload Agent

Currently, workload agents can be configured to connect to an HTTP proxy only through the environment variable `SYSDIG_EXTRA_CONF` to be provided to the instrumented container.

The following configuration options affect the behavior of the HTTP proxy setting, and can be specified either in JSON or YAML format under `http_proxy` heading.
Refer to [Enable HTTP proxy for agents](/en/enable-http-proxy-for-agents) for further details.

| Option                   | Default | Description           |
|--------------------------|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `proxy_host`             | ``      | Indicates the hostname of the proxy server. The default is an empty string, which implies communication through an HTTP proxy is disabled.              |
| `proxy_port`             | ``      | Specifies the port on the proxy server the agent should connect to. The default is 0, which indicates that the HTTP proxy is disabled.                  |
| `proxy_user`             | ``      | Required if HTTP authentication is configured. This option specifies the username for the HTTP authentication. The default is an empty string, which indicates that authentication is not configured.       |
| `proxy_password`         | ``      | Required if HTTP authentication is configured. This option specifies the password for the HTTP authentication. The default is an empty string. Specifying `proxy_user` with no `proxy_password` is allowed. |
| `ssl`                    | `false` | Default: `false`. If set to `true`, the connection between the agent and the proxy server is encrypted.                       |
| `ssl_verify_certificate` | `true`  | Determines whether the agent will verify the certificate presented by the proxy.                    |
| `ca_certificate`         | `true`  | The path to the CA certificate for the proxy server. If `ssl_verify_certificate` is enabled, the CA certificate must be signed appropriately.           |

For example, the following configuration shows how to configure the Workload Agent to connect to the HTTP proxy with:
 - `squid.my.domain.com:6444` as a host and port;
 - `my-user` as a username;
 - `my-proxy-password` as a password.

```yaml
http_proxy:
  proxy_host: squid.my.domain.com
  proxy_port: 6443
  proxy_user: my-user
  proxy_password: my-proxy-password
```

Note that newlines and spaces matter when passing such a configuration as a YAML to the Workload Agent.

```shell
SYSDIG_EXTRA_CONF="http_proxy:\n  proxy_host: squid.my.domain.com\n  proxy_port: 6443\n  proxy_user: my-user\n  proxy_password: my-proxy-password"
```

This options supports cleartext passwords only.
