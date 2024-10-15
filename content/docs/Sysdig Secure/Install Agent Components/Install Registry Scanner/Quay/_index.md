---
linkTitle: "Quay.io"
title: "Quay.io"
weight: "13"
no_list: true
aliases:
  - /en/quay-registry-scan/
---

To run the scanner in your Quay.io repository, you need the user and password with `admin` permissions.

```bash
$ helm upgrade --install registry-scanner sysdig/registry-scanner --version=1 \
--set config.secureBaseURL=<SYSDIG_SECURE_URL> \
--set config.secureAPIToken=<SYSDIG_SECURE_API_TOKEN> \
--set config.registryType=quay \
--set config.registryURL=quay.io \
--set config.registryUser=<QUAY_USER> \
--set config.registryPassword=<QUAY_PASSWORD>
```

- `<QUAY_USER>`: CLI user for Quay.io.
- `<QUAY_PASSWORD>`: CLI password for Quay.io. It must be generated through the account settings.
