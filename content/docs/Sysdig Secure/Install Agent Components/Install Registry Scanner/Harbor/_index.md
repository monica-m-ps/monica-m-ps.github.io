---
linkTitle: "Harbor"
title: "Harbor"
weight: "13"
no_list: true
aliases:
  - /en/harbor/
  
---

To run the scanner in Harbor, you must have the registry URL and the user credentials with the Administrator profile.

```bash
$ helm upgrade --install registry-scanner sysdig/registry-scanner --version=1 \
--set config.secureBaseURL=<SYSDIG_SECURE_URL> \
--set config.secureAPIToken=<SYSDIG_SECURE_API_TOKEN> \
--set config.registryType=harbor \
--set config.registryURL=<HARBOR_REGISTRY_URL> \
--set config.registryUser=<HARBOR_USER> \
--set config.registryPassword=<HARBOR_PASSWORD>
```

- `<HARBOR_USER>` \ `<HARBOR_PASSWORD>`: User credentials with `Administrator` flag activated (`Project Admin` role is not currently supported)
- `<HARBOR_REGISTRY_URL>`: Harbor registry URL <br> e.g., `core.harbor.domain`

