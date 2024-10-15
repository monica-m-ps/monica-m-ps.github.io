---
linkTitle: "Configuration Library"
title: "Configuration Library"
weight: "11"
no_list: true
aliases:
  - /en/configuration-library.html
  - /en/configuration-library
Description: "The Sysdig configuration library lists all the major configurations supported by Sysdig agent components. This  document is evolving and will be updated as new configurations are added to the product."
---

## General Agent Configuration

The configuration parameters outlined in this section apply to both Sysdig Monitor and Sysdig Secure.

### Cluster

Identifier for the Kubernetes cluster where you install the agent. For more information, see [Agent Configuration](/en/understand-the-agent-configuration).

dragent.yaml : `k8s_cluster_name`

Helm: `global.clusterConfig.name`

For example, `ec2_cluster`.

### Access Key

See <a href="/en/docs/administration/administration-settings/agent-installation-overview-and-key/#retrieve-the-agent-access-key">Sysdig Agent Access Keys</a> to learn how to retrieve the agent keys.

dragent.yaml : `customerid`

Helm: `global.sysdig.accessKey`

### Secret

The name of a Kubernetes secret containing an access-key entry.

This configuration does not exist in the dragent.yaml.

Helm: `global.sysdig.accessKeySecret`

### Region

The SaaS region where the agent is installed. See <a href="/en/docs/administration/saas-regions-and-ip-ranges/">Regions and IP Ranges</a> for more information.

This configuration does not exist in the dragent.yaml.

Helm: `global.sysdig.region`

Possible values include: <code>us1</code>, <code>us2</code>, <code>us3</code>, <code>us4</code>, <code>eu1</code>, <code>au1</code>, and <code>custom</code>.

### Global Tags

Sets the global tags which can override agent tags. See <a href="/en/docs/installation/">Quick Install Sysdig Agent</a> for more information.

dragent.yaml: `tags`
Helm: `global.sysdig.tags`

### Agent Tags

The list of tags to identify the host where the agent is installed.  See <a href="/en/docs/installation/">Quick Install Sysdig Agent</a> for more information.

dragent.yaml: `tags`
Helm: `global.sysdig.tags`

For example: <code>role:webserver</code>, <code>location:europe</code>, <code>role:webserver</code>.

### Proxy

Allows the agent to communicate with Sysdig collector through a <code>http_proxy</code>. See <a href="/en/enable-http-proxy-for-agents/">Enable HTTP Proxy for Agents</a> for more information.

dragent.yaml: `http_proxy`
Helm: `global.proxy.httpProxy`

### HTTP Proxy Host

The host IP of the proxy server.

dragent.yaml: `http_proxy.proxy_host`

### HTTP Proxy Port

See <a href="/en/enable-http-proxy-for-agents">Enable HTTP Proxy for Agents</a> for more information. 

dragent.yaml: `http_proxy.proxy_port`, `http_proxy.proxy_user`, `http_proxy.proxy_password`, `http_proxy.ssl`, `http_proxy.ssl_verify_certificate`, `http_proxy.ca_certificate`.

### Collector

Enter the hostname or IP address of the Sysdig collector service. Note that when used within <code>dragent.yaml</code>, must be lowercase collector.</p><p>See <a href="/en/docs/administration/on-premises-deployments/on-premises-installation/installer-kubernetes-openshift/">On-Premises Installation</a> for more information.

Helm: `collectorSettings.collectorHost`

### Collector Port

On-prem only. The port used by the Sysdig collector service.

Port: `6443`

### eBPF

This configuration does not exist in the dragent.yaml.

In Helm:

- Set `ebpf.enabled` to `true` to enable the agent Universal eBPF or the current eBPF driver. The default is `false`.

- Set `ebpf.kind` to `universal_ebpf` to enable the Universal eBPF driver. Set to `legacy_ebpf` to enable the eBPF driver.

  Note `ebpf.enabled` must also be set to true for this configuration to work.

### FIPS Mode

dragent.yaml: `fips_mode`


Optional: Set to `true` for the agent to use a FIPS-validated crypto module to encrypt the communication between the agent and the Sysdig backend. The agent will log `FIPS mode is enabled` if a FIPS-validated crypto module was successfully loaded.

The default is `false`.

### OpenSSL Library Location

dragent.yaml: `openssl_lib`

Agent version 12.16.x and older: Required when <code>fips_mode</code> is set to <code>true</code>.  Path to the directory containing user-provided OpenSSL v1.1.1 shared library files: <code>libcrypto.so.1</code>, and <code>libssl.so.1</code>.  User-provided OpenSSL libraries must contain a FIPS-validated crypto module if setting <code>fips_mode</code> to <code>true</code>.</p>

Agent version 12.17.0 and newer: <p>Optional: Path to the directory containing user-provided OpenSSL v3.x shared library files: <code>libcrypto.so.3</code>, and <code>libssl.so.3</code>.  User-provided OpenSSL libraries must contain a FIPS-validated crypto module if setting <code>fips_mode</code> to <code>true</code>.</p>

By default, the agent uses bundled OpenSSL shared libraries.

### OpenSSL Configuration File Location

dragent.yaml: `openssl_conf`

Agent version 13.0 and newer: <p>Required when <code>openssl_lib</code> is used to point the agent to a custom OpenSSL v3.x library.  If <code>fips_mode</code> is set to <code>true</code>, the configuration file specified by <code>openssl_conf</code> must contain the properties specified in the "Making all applications use the FIPS module by default" section of <a href="https://www.openssl.org/docs/man3.2/man7/fips_module.html">fips_module(7) man page.</a> If the <code>OPENSSL_CONF</code> environment variable is also set, it will take precedence over the <code>openssl_conf</code> value.</p>

By default, the agent uses OpenSSL configuration files included with its bundled libraries.

### Instance Metadata Service (IMDS)

dragent.yaml: `imds_version`

Optional: Enables token-based communication with the Amazon Web Service (AWS) metadata service IMDSv2.

The default is `1`.

However, the agent internally upgrades the IMDS version to IMDSv2 when the IMDSv1 API call returns a "Not Authorized" error. You can ignore the INFO level message stating to change the configuration to `2`.

### Learn More

- [Agent Configuration for Secure](/en/configuration-library-secure)
- [Agent Configuration for Monitor](/en/configuration-library-monitor)

  
