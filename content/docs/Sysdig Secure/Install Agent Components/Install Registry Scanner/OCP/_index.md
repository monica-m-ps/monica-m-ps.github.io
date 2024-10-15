---
linkTitle: "OpenShift Container Platform Registry"
title: "OpenShift Container Platform Registry"
weight: "13"
no_list: true
aliases:
  - /en/ocp-registry-scan/
description: "This topic helps you set up Sysdig Registry Scanner on OpenShift Container Platform (OCP) Registry. See [Registry](/en/vuln-registry-scan/) to review the scan results in the Sysdig Secure UI."
---
Ensure that your system meets the [requirements](/en/install-registry-scan/#prerequisites).

To run the scanner in your OCP Registry, you need the user and password with `admin` permissions.

```bash
$ helm upgrade --install registry-scanner sysdig/registry-scanner --version=1 \
--set config.secureBaseURL=<SYSDIG_SECURE_URL> \
--set config.secureAPIToken=<SYSDIG_SECURE_API_TOKEN> \
--set config.registryType=ocp\
```
