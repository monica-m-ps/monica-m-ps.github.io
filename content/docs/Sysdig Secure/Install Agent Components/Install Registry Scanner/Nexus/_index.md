---
linkTitle: "Nexus"
title: "Nexus"
weight: "13"
no_list: true
aliases:
  - /en/nexus-scan/
---

To run the scanner in Nexus, you must have the registry URL and the user credentials with a custom role that includes a single permission: `nx-repository-view-docker-*-read`.

```bash
$ helm upgrade --install registry-scanner sysdig/registry-scanner --version=1 \
--set config.secureBaseURL=<SYSDIG_SECURE_URL> \
--set config.secureAPIToken=<SYSDIG_SECURE_API_TOKEN> \
--set config.registryType=nexus \
--set config.registryURL=<NEXUS_REGISTRY_URL> \
--set config.registryUser=<NEXUS_USER> \
--set config.registryPassword=<NEXUS_PASSWORD>
```

- `<NEXUS_USER>` \ `<NEXUS_PASSWORD>`: User credentials with `nx-admin` role assigned.
- `<NEXUS_REGISTRY_URL>`: Nexus registry URL <br> for example, `nexus.your.domain`

