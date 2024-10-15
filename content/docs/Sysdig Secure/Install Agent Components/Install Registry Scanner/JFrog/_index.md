---
linkTitle: "JFrog Artifactory Configuration"
title: "JFrog Artifactory Configuration"
weight: "11"
no_list: true
aliases:
  - /en/identify-jfrog-artifactory-configuration
description: "This section helps you set up JFrog artifactory for registry scanning. This includes identifying your Artifactory configuration prior to installing the Registry Scanner suitable for your environment. JFrog On-Prem has three configuration options: `subdomain`, `repository path`, and `port`."
---

## Prerequisites

Perform a pre-installation check to [identify your  Artifactory configuration](/en/identify-jfrog-artifactory-configuration). Identify the following configuration parameters:

- `registryURL`: The Registry URL corresponding to your Artifactory environment.
- `registryApiUrl`: The Registry API URL corresponding to your Artifactory environment.

## Installation

Connect with an externally managed version of Jfrog Artifactory.

```bash
$ helm upgrade --install registry-scanner sysdig/registry-scanner --version=1 \
--set config.secureBaseURL=<SYSDIG_SECURE_URL> \
--set config.secureAPIToken=<SYSDIG_SECURE_API_TOKEN> \
--set config.registryType=artifactory \
--set config.registryUser=<JFROG_ARTIFACTORY_USER> \
--set config.registryPassword=<JFROG_ARTIFACTORY_PASSWORD>
--set config.registryURL=<JFROG_ARTIFACTORY_REGISTRY_URL> \
--set config.registryApiUrl=<JFROG_ARTIFACTORY_REGISTRY_API_URL>
```

#### Limitations

Multi-arch Docker manifest/packages are not supported.

## Artifactory Cloud Versions 5.x and Above

### Verify Your Artifactory Setup

If your server name is `my-server` (`my-server.jfrog.io`), your registry (Artifactory Docker repo) is `my-repo`, and you pull images using the following pattern:

```
docker pull my-server.jfrog.io/my-repo/image-name
```

You can confirm that you are using an Artifactory Cloud versions that is 5.x or above.
Alternatively, you can also check your Artifactory version following the instructions at [JFrog Artifactory](https://jfrog.com/help/r/how-can-i-check-artifactory-version-number-on-my-artifactory-saas-account).
### Registry URL

`my-server.jfrog.io/my-repo`

### Registry API URL

`my-server.jfrog.io/artifactory/api/docker/my-repo`


## Artifactory Cloud Versions 4.x and Below

### Verify Your Artifactory Setup

If your server name is `my-server` (`my-server.jfrog.io`), your registry (Artifactory Docker repo) is `my-repo`, and you pull images using the following pattern:

```
docker pull my-server-my-repo-name.jfrog.io/image-name
```

You can confirm that you are using an Artifactory Cloud version that is v4.x or below.
You can also check your Artifactory version following the instructions at https://jfrog.com/help/r/how-can-i-check-artifactory-version-number-on-my-artifactory-saas-account
### Registry URL

`my-server-my-repo.jfrog.io`

### Registry API URL

`my-server.jfrog.io/artifactory/api/docker/my-repo`

## Artifactory On-Prem Subdomain

### Verify Your Artifactory Setup

If your server name is `host.mycompany.com`, your registry (Artifactory Docker repo) is `my-repo`, and you pull images using the following pattern:

```
docker pull my-repo.host.mycompany.com/image-name
```

You can confirm that you are on an Artifactory On-Prem version using the Subdomain method.

### Registry URL

`my-repo.host.mycompany.com`

### Registry API URL

`host.mycompany.com/artifactory/api/docker/my-repo`

## Artifactory On-Prem Repository Path

### Verify Your Artifactory Setup

If your server name is `host.mycompany.com`, your registry (Artifactory Docker repo) is `my-repo`, and you pull images using the following pattern:

```
docker pull host.mycompany.com/my-repo/image-name
```

You can confirm that you are on an Artifactory on-prem version using the repository path method.

### Registry URL

`host.mycompany.com/my-repo`

### Registry API URL

`host.mycompany.com/artifactory/api/docker/my-repo`

## Artifactory On-Prem Port

### Verify Your Artifactory Setup

If your server name is `host.mycompany.com`, your registry (Artifactory Docker repo) is `my-repo`, and you pull images using the following pattern:

```
docker pull host.mycompany.com:5000/image-name
```

You can confirm that you are on an Artifactory on-prem version using the port method.

You will have a port dedicated to `my-repo`.

### Registry URL

`host.mycompany.com:5000`

### Registry API URL

`host.mycompany.com:5000/artifactory/api/docker/my-repo`
