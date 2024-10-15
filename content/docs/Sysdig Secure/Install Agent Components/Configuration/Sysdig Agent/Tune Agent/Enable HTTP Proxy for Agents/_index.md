---
linkTitle: "Enable HTTP Proxy for Agents"
title: "Enable HTTP Proxy for Agents"
weight: "12"
aliases:
  - /en/enable-http-proxy-for-agents.html
  - /en/enable-http-proxy-for-agents
  - /en/docs/installation/sysdig-agent/agent-configuration/enable-http-proxy-for-agents/
  - /en/docs/installation/sysdig-agent/agent-configuration/tune-agent/enable-http-proxy-for-agents/
---

You can configure the agent to allow it to communicate with the Sysdig
collector through an HTTP proxy. HTTP proxy is usually configured to
offer greater visibility and better management of the network.

## Agent Behaviour

The agent can connect to the collector through an HTTP proxy by sending
an HTTP CONNECT message and receiving a response. The proxy then
initiates a TCP connection to the collector. These two connections form
a tunnel that acts like one logical connection.

By default, the agent will encrypt all messages sent through this
tunnel. This means that after the initial CONNECT message and response,
all the communication on that tunnel is encrypted by SSL end-to-end.
This encryption is controlled by the top-level `ssl` parameter in the
agent configuration.

Optionally, the agent can add a second layer of encryption, securing the
CONNECT message and response. This second layer of encryption may be
desired in the case of HTTP authentication if there is a concern that
network packet sniffing could be used to determine the user's
credentials. This second layer of encryption is enabled by setting the
`ssl` parameter to true in the `http_proxy` section of the agent
configuration. See
[Examples](#examples)
for details.

## Configuration

You specify the following parameters at the same level as `http_proxy`
in the `dragent.yaml` file. These existing configuration options affect
the communication between the agent and collector (both with and without
a proxy).

-   `ssl`: Default: `true`. It is not recommended to change this setting. If set to `false`, the metrics sent from the agent to the collector are unencrypted.

-   `ssl_verify_certificate`: Determines whether the agent verifies the
    SSL certificate sent from the collector (default is `true`).

The following configuration options affect the behavior of the HTTP
Proxy setting. You specify them under the `http_proxy` heading in the
`dragent.yaml` file.

-   `proxy_host`: Indicates the hostname of the proxy server. The
    default is an empty string, which implies communication through an
    HTTP proxy is disabled.

-   `proxy_port`: Specifies the port on the proxy server the agent
    should connect to. The default is 0, which indicates that the HTTP
    proxy is disabled.

-   `proxy_user` : Required if HTTP authentication is configured. This
    option specifies the username for the HTTP authentication. The
    default is an empty string, which indicates that authentication is
    not configured.

-   `proxy_password` : Required if HTTP authentication is configured.
    This option specifies the password for the HTTP authentication. The
    default is an empty string. Specifying `proxy_user` with no
    `proxy_password` is allowed.

-   `ssl`: Default: `false`.  If set to true, the connection between the agent and the
    proxy server is encrypted.

    Note that this parameter requires the top-level `ssl` parameter to
    be enabled, as the agent does not support SSL to the proxy but
    unencrypted traffic to the collector. This additional security
    prevents you from misconfiguring the agent assuming the metrics are
    as well encrypted end-to-end when they are not.

-   `ssl_verify_certificate`: Determines whether the agent will verify
    the certificate presented by the proxy.

    This option is configured independently of the top-level
    `ssl_verify_certificate` parameter. This option is enabled by
    default. If the provided certificate is not correct, this option can
    cause the connection to the proxy server to fail.

-   `ca_certificate`: The path to the CA certificate for the proxy
    server. If `ssl_verify_certificate` is enabled, the CA certificate
    must be signed appropriately.

## Examples

### SSL Between Proxy and Collector

In this example, SSL is enabled only between the proxy server and the
collector.

```yaml
collector_port: 6443
ssl: true
ssl_verify_certificate: true
http_proxy:
  proxy_host: squid.yourdomain.com
  proxy_port: 3128
```

### SSL

The following example shows SSL is enabled between the agent and the
proxy server as well as between the proxy server and the collector.

```yaml
collector_port: 6443
ssl: true
http_proxy:
  proxy_host: squid.yourdomain.com
  proxy_port: 3129
  ssl: true
  ssl_verify_certificate: true
  ca_certificate: /usr/proxy/proxy.crt
```

### SSL with Username and Password

The following configuration instructs the agent to connect to a proxy
server located at `squid.yourdomain.com` on port `3128`. The agent will
request the proxy server to establish an HTTP tunnel to the Sysdig
collector at `collector-your.sysdigcloud.com` on port 6443. The agent
will authenticate with the proxy server using the given user and
password combination.

```yaml
collector: collector-your.sysdigcloud.com
collector_port: 6443
http_proxy:
  proxy_host: squid.yourdomain.com
  proxy_port: 3128
  proxy_user: sysdig_customer
  proxy_password: 12345
  ssl: true
  ssl_verify_certificate: true
  ca_certificate: /usr/proxy/proxy_cert.crt
