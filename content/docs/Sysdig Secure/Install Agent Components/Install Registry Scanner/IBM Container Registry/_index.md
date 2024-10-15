---
linkTitle: "IBM Container Registry"
title: "IBM Container Registry Scanner"
weight: "13"
no_list: true
aliases:
   - /en/ibm-container-registry-scan/
---

To run the scanner in IBM Container Registry (ICR), you must have the URL, an API key, and the account ID of the registry.

```bash
$ helm upgrade --install registry-scanner sysdig/registry-scanner --version=1 \
--set config.secureBaseURL=<SYSDIG_SECURE_URL> \
--set config.secureAPIToken=<SYSDIG_SECURE_API_TOKEN> \
--set config.registryType=icr \
--set config.registryURL=<ICR_REGISTRY_URL> \
--set config.registryUser=iamapikey \
--set config.registryPassword=<ICR_APIKEY> \
--set config.registryAccountId=<ICR_ACCOUNT_ID>

```

- `<ICR_REGISTRY_URL>`: The ICR registry URL. <br> For example,  `https://us.icr.io`
- `<ICR_ACCOUNT_ID>`: The ICR account ID where the registry is located.

