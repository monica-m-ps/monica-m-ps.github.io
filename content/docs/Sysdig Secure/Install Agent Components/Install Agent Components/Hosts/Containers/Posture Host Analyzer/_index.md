---
linkTitle: "Posture Host Analyzer"
title: "Posture Host Analyzer (Non-Kubernetes)"
weight: "12"
no_list: true
aliases:
  - /en/install-container-posture-host-analyzer/
  - /en/install-container-posture-host-analyzer
description: "This page describes how to install the Sysdig Posture Host Analyzer on non-Kubernetes hosts using containers. This is used to scan for compliance violations on stand-alone hosts."
---

{{% callout type="note" %}}

Posture Host Analyzer is not yet supported in on-premises environments. For on-prem environments, consider using the [Legacy Compliance](/en/docs/sysdig-secure/posture/compliance/legacy-versions/compliance-unified/) module.

{{% /callout %}}

## Prerequisites

- Retrieve your [access key](/en/agent-access-key/) to use for `ACCESS_KEY`.
- Retrieve your Sysdig Secure [endpoint by region](/en/regions-ranges/) to use for `API_ENDPOINT`.
- Docker installed.
- Any Linux distribution.
- Supported CPU Architectures:
   - x86_64

## CIS Benchmarks

We provide specific CIS benchmarks for the following Linux distributions:
- Bottlerocket
- Distribution Independent Linux
- Red Hat Enterprise Linux (RHEL) 8
- Red Hat Enterprise Linux 9
- Rocky Linux 9
- Ubuntu 20.04 LTS
- Ubuntu 22.04 LTS

For other Linux distributions, we provide Distribution Independent Linux CIS benchmarks.

## Install Posture Analyzer

Run the following command to deploy the non-Kubernetes Posture Analyzer on a host as a container:

```
sudo docker run -d [-e TAGS=<TAGS>] -v /:/host:ro -v /tmp:/host/tmp --privileged --network host --pid host --env ACCESS_KEY="xxx"  --env API_ENDPOINT=secure.sysdig.com quay.io/sysdig/kspm-analyzer:latest
```

* Replace `XXX` with your agent access key, and `<sysdig-secure-endpoint>` with the URL for your [Sysdig Secure endpoint by region](/en/regions-ranges/).
* If proxy is used, pass the proxy settings by using the following flags:

   `-e HTTPS_PROXY=squid.yourdomain.com:PORT`
    `-e HTTP_PROXY=squid.yourdomain.com:PORT`

### Options

#### Zones

To apply posture with specific zones, the agent and the Kubernetes Security Posture Management (KSPM) components must be in the stand-alone Docker host. Include the `kspm-analyzer` container as part of the `node-analyzer` pod while installing to add standalone Docker hosts in the Host list/ Zones.

Note that Inventory is based on Cloud Security Posture Management (CSPM)resources.

#### Add Agent Tags

Optionally, to add agent tags, use the environment variable `-e TAGS=<your_agent_tags>`. 

Replace <your_agent_tags> with the tags you wish to add. For example:

```
-e TAGS=myApp.Env:Production,host.name:MyHost
```

### Results

Once the container is running, the analyzer will begin scanning your host for compliance violations and providing security recommendations. To view the results in UI, log in to Sysdig Secure, and go to **Posture** > **Compliance**

Results will be shown within a few minutes of installation and scans are refreshed every 24 hours.
