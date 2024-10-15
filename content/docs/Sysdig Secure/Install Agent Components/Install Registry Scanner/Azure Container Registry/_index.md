---
linkTitle: "Azure Container Registry"
title: "Azure Container Registry"
weight: "13"
no_list: true
aliases:
  - /en/azure-container-registry/
---

### Azure Container Registry

Use one of the following installation options, depending on the credential format used:

#### Authentication Using Registry Token

To run the scanner in Azure Container Registry (ACR) using a registry token, you must have a token with `_repositories_admin` scope map to read the full registry.

```bash
$ helm upgrade --install registry-scanner sysdig/registry-scanner --version=1 \
--set config.secureBaseURL=<SYSDIG_SECURE_URL> \
--set config.secureAPIToken=<SYSDIG_SECURE_API_TOKEN> \
--set config.registryType=acr \
--set config.registryURL=<ACR_REGISTRY_URL> \
--set config.registryUser=<ACR_TOKEN_NAME> \
--set config.registryPassword=<ACR_TOKEN_PASSWORD>
```

- `<ACR_REGISTRY_URL>`: The Azure registry URL <br> For example, `testregistryscanner.azurecr.io`
- `<ACR_TOKEN_NAME>`: The Azure registry token name
- `<ACR_TOKEN_PASSWORD>`: The Azure registry token password

##### Limitations

The registry token must use the  `_repositories_admin` scope map.
For more information, see the [Azure ACR documentation.](https://learn.microsoft.com/en-us/azure/container-registry/container-registry-repository-scoped-permissions)

#### Authentication by Service Principal

To run the scanner in Azure Container Registry (ACR) using Service Principal, follow the [Azure ACR instructions](https://learn.microsoft.com/en-us/azure/container-registry/container-registry-auth-service-principal) to create Service Principal with `acrpull` role and scoped to your registry.

```bash
$ helm upgrade --install registry-scanner sysdig/registry-scanner --version=1 \
--set config.secureBaseURL=<SYSDIG_SECURE_URL> \
--set config.secureAPIToken=<SYSDIG_SECURE_API_TOKEN> \
--set config.registryType=acr \
--set config.registryURL=<ACR_REGISTRY_URL> \
--set config.registryUser=<SP_ID> \
--set config.registryPassword=<SP_PASSWORD>
```

- `<ACR_REGISTRY_URL>`: The Azure registry URL. <br> For example, `testregistryscanner.azurecr.io`
- `<SP_ID>`: The Azure Service Principal ID.
- `<SP_PASSWORD>`: The Azure Service Principal password.
