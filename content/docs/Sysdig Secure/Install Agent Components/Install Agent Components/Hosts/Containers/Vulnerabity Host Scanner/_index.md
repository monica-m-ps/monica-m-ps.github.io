---
linkTitle: "Vulnerability Host Scanner"
title: "Vulnerability Host Scanner"
weight: "11"
no_list: true
aliases:
  - /en/install-container-host-scanner/
  - /en/install-container-vuln-host-scan
description: "This page describes how to install the Sysdig Vulnerability Host Scanner on non-Kubernetes hosts using containers. This Host Scanner is used to scan for vulnerabilities on hosts, in addition to the default runtime scanner on containers."
---

## Prerequisites

- Retrieve your [access key](/en/agent-access-key/) to use for `SYSDIG_ACCESS_KEY=<your-access-key>`
- Check your Sysdig Secure [endpoint by region](/en/regions-ranges/) to use for `SYSDIG_API_URL=https://<sysdig-url>`
- See [Host Scanner installation requirements](/en/install-reqs-host-scan) for remaining requirements.

## Installation

Run the following Docker command to deploy the Sysdig Host Scanning container:

```
docker run --detach -e HOST_FS_MOUNT_PATH=/host -e SYSDIG_ACCESS_KEY=<access-key> -e SYSDIG_API_URL=<sysdig-secure-endpoint> -e SCAN_ON_START=true -v /:/host:ro -v /var/run:/host/var/run:ro --uts=host --net=host quay.io/sysdig/vuln-host-scanner:$(curl -L -s https://download.sysdig.com/scanning/sysdig-host-scanner/latest_version.txt)
```

This command downloads and starts the Sysdig Host Scanning container.

- Replace *<access-key>* with your agent access key, and *<sysdig-secure-endpoint>* with the URL for your Sysdig Secure endpoint by region.

Once the container is running, the scanner will begin scanning your host for vulnerabilities and providing security recommendations. You can view the results in the Sysdig Secure UI within 12 hours of installation as scans are refreshed every 12 hours.

### Option: Scan for Containers

You can extend the host scanner to scan for containers such as Docker and Podman.

See [Container Scanning](/en/container-scan) for details.

## Next Steps

[Use the Vuln Host Scanner in the Sysdig Secure UI](/en/docs/sysdig-secure/vulnerabilities/findings/runtime/host-scanning/)
