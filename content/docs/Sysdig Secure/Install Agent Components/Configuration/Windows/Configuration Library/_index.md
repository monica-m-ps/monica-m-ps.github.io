---
linkTitle: "Configuration Library"
title: "Configuration Library"
weight: "11"
aliases:
  - /en/configuration-library-windows.html
  - /en/configuration-library-windows
Description: "The Sysdig configuration library lists all the major configurations supported by the Sysdig Windows Agent. This  document is evolving and will be updated as new configurations are added to the product."
---

## Sysdig Windows Agent

### Generic Configuration

<table>
    <tr>
        <td>Configuration</td>
        <td>dragent.yaml</td>
        <td>Description</td>
        <td>Default</td>
    </tr>
    <tr>
        <td>Access Key</td>
        <td><code>customerid</code></td>
        <td>
            <p>See <a href="/en/docs/administration/administration-settings/agent-installation-overview-and-key/#retrieve-the-agent-access-key">Sysdig Agent Access Keys</a> to learn how to retrieve the agent keys.</p>
        </td>
        <td></td>
    </tr>
    <tr>
        <td>Agent Tags</td>
        <td><code>tags</code></td>
        <td>
            The list of tags to identify the host where the agent is installed. For example: <code>role:webserver</code>, <code>location:europe</code>, <code>role:webserver</code>. See
            <a href="/en/docs/installation/windows/">Quick Install Sysdig Windows Agent</a> for more information.
        </td>
        <td></td>
    </tr>
    <tr>
        <td>Proxy</td>
        <td><code>http_proxy</code></td>
        <td>
            <p>Allows the agent to communicate with Sysdig collector through <code>http_proxy</code>. See <a href="/en/enable-http-proxy-for-agents/">Enable HTTP Proxy for Agents</a> for more information.</p>
        </td>
        <td></td>
    </tr>
    <tr>
        <td>HTTP Proxy Host</td>
        <td><code>http_proxy.proxy_host</code></td>
        <td><p>The host IP of the proxy server.</p></td>
        <td></td>
    </tr>
    <tr>
        <td>HTTP Proxy Port</td>
        <td><code>http_proxy.proxy_port</code></td>
        <td>
            <p>See <a href="/en/enable-http-proxy-for-agents">Enable HTTP Proxy for Agents</a> for more information.</p>
        </td>
        <td></td>
    </tr>
    <tr>
        <td>HTTP Proxy User</td>
        <td><code>http_proxy.proxy_user</code></td>
        <td>
            <p>See <a href="/en/enable-http-proxy-for-agents">Enable HTTP Proxy for Agents</a> for more information.</p>
        </td>
        <td></td>
    </tr>
    <tr>
        <td>HTTP Proxy Password</td>
        <td><code>http_proxy.proxy_password</code></td>
        <td>
            <p>See <a href="/en/enable-http-proxy-for-agents">Enable HTTP Proxy for Agents</a> for more information.</p>
        </td>
        <td></td>
    </tr>
    <tr>
        <td>Enable HTTP Proxy</td>
        <td><code>http_proxy.ssl</code></td>
        <td>
            <p>See <a href="/en/enable-http-proxy-for-agents">Enable HTTP Proxy for Agents</a> for more information.</p>
        </td>
        <td></td>
    </tr>
    <tr>
        <td>HTTP Proxy SSL verification</td>
        <td><code>http_proxy.ssl_verify_certificate</code></td>
        <td>
            <p>See <a href="/en/enable-http-proxy-for-agents">Enable HTTP Proxy for Agents</a> for more information.</p>
        </td>
        <td></td>
    </tr>
    <tr>
        <td>HTTP Proxy CA certificate</td>
        <td><code>http_proxy.ca_certificate</code></td>
        <td>
            <p>See <a href="/en/enable-http-proxy-for-agents">Enable HTTP Proxy for Agents</a> for more information.</p>
        </td>
        <td></td>
    </tr>
    <tr>
        <td>Collector endpoint</td>
        <td><code>collector</code></td>
        <td>
            <p>Enter the host name or IP address of the Sysdig collector service. Note that when used within <code>dragent.yaml</code>, must be lowercase collector.</p>
            <p>See <a href="/en/docs/administration/on-premises-deployments/on-premises-installation/installer-kubernetes-openshift/">On-Premises Installation</a> for more information.</p>
        </td>
        <td></td>
    </tr>
    <tr>
        <td>Collector Port</td>
        <td><code>collector_port</code></td>
        <td>On-prem only. The port used by the Sysdig collector service.</td>
        <td>6443</td>
    </tr>
     <tr>
        <td>Event capture settings</td>
        <td><code>windows</code></td>
        <td>Controls various internal configuration knobs that influence the variety of captured events</td>
        <td></td>
    </tr>
    <tr>
        <td>Enable thread events</td>
        <td><code>windows.enable_thread</code></td>
        <td>Controls if thread events are captured</td>
        <td>true</td>
    </tr>
     <tr>
        <td>Enable module events</td>
        <td><code>windows.enable_image</code></td>
        <td>Controls if image loading/unloading events are captured</td>
        <td>true</td>
    </tr>
    <tr>
        <td>Enable network events</td>
        <td><code>windows.enable_network</code></td>
        <td>Controls if network events are captured</td>
        <td>true</td>
    </tr>
    <tr>
        <td>Enable file events</td>
        <td><code>windows.enable_file</code></td>
        <td>Controls if file system events are captured</td>
        <td>true</td>
    </tr>
    <tr>
        <td>Enable registry events</td>
        <td><code>windows.enable_registry</code></td>
        <td>Controls if registry events are captured</td>
        <td>true</td>
    </tr>
    <tr>
        <td>Enable handle events</td>
        <td><code>windows.enable_handle</code></td>
        <td>Controls if object manager events are captured</td>
        <td>false</td>
    </tr>
    <tr>
        <td>Enable Audit API events</td>
        <td><code>windows.enable_audit_api</code></td>
        <td>Controls if Audit API events are captured</td>
        <td>true</td>
    </tr>
    <tr>
        <td>Enable AMSI events</td>
        <td><code>windows.enable_amsi_scan_interface</code></td>
        <td>Controls if Antimalware Scan Interface events are captured</td>
        <td>true</td>
    </tr>
</table>
