---
linkTitle: "Google Registry"
title: "Google Registry"
weight: "13"
no_list: true
aliases:
  - /en/google-registry
  - /en/docs/installation/sysdig-secure/install-registry-scanner/google-registry/
---

## Google Container Registry

### Prerequisites

- A [Service Account](https://cloud.google.com/iam/docs/service-accounts-create#creating) within one of the projects is created. 

-  The required roles of `Project Viewer`  is [assigned](https://cloud.google.com/iam/docs/granting-changing-revoking-access#multiple-roles-console) to that service account.

### Install Registry Scanner

Run the Registry Scanner as follows:

```bash
$ helm repo add sysdig https://charts.sysdig.com
$ helm repo update
$ helm upgrade --install registry-scanner sysdig/registry-scanner --version=1 \
--set config.secureBaseURL=<SYSDIG_SECURE_URL> \
--set config.secureAPIToken=<SYSDIG_SECURE_API_TOKEN> \
--set config.registryType=gcr \
--set config.registryURL=<GCR_REGISTRY_URL> \
--set-file config.registryPassword=<GCR_SERVICE_ACCOUNT_KEY>

```

- `<GCR_SERVICE_ACCOUNT_KEY>`: The path to an unencoded Service Account JSON [access key](https://cloud.google.com/artifact-registry/docs/docker/authentication#json-key). 

   For example, `sa-3b0c7dec5ea0.json`

- `<GCR_REGISTRY_URL>`: The Google Container Registry URL.

   For example, `gcr.io` or `us.gcr.io`

## Google Artifact Registry

### Prerequisites

- A [Service Account](https://cloud.google.com/iam/docs/service-accounts-create#creating) within one of the projects is created. 

-  The required role `Artifact Registry Reader` is [assigned](https://cloud.google.com/iam/docs/granting-changing-revoking-access#multiple-roles-console) to that service account.

### Install Registry Scanner

You can set up the Registry Scanner either at the project level or the organization level.

```bash
$ helm repo add sysdig https://charts.sysdig.com
$ helm repo update
$ helm upgrade --install registry-scanner sysdig/registry-scanner --version=1 \
--set config.secureBaseURL=<SYSDIG_SECURE_URL> \
--set config.secureAPIToken=<SYSDIG_SECURE_API_TOKEN> \
--set config.registryType=gar \
--set config.registryURL=<GAR_REGISTRY_URL> \
--set config.registryPassword=<GAR_REGISTRY_PASSWORD>

```

- `<GAR_REGISTRY_PASSWORD>`: Base64 encoded Service Account JSON [access key](https://cloud.google.com/artifact-registry/docs/docker/authentication#json-key). 

  To encode JSON key file to base64, use the following command: `--set config.registryPassword="$(cat <GAR_SA_FILE_NAME>.json | base64)"`

- `<GAR_REGISTRY_URL>`: Google Artifact Registry URL.

   For example, `us-docker.pkg.dev`

### Create a Custom Role

If you prefer not to assign the `Project Viewer` role to the Service Account, you can create a custom role with restricted permissions. The [Custom Role](https://cloud.google.com/iam/docs/creating-custom-roles#creating)  should include only the necessary permission, such as `resourcemanager.projects.get`. This permission allows the Service Account to list repositories on the Docker v2 `_catalog` endpoint. 

### Create a Custom Role at Project Level

```yaml
gcloud iam roles create sysdig.repositorylist --project==<YOUR_PROJECT_ID> \
    --title="Sysdig - Artifact Registry - List Repositories" \
    --description="Sysdig - Artifact Registry - List Repositories on dockerv2 _catalog API endpoint" \
    --permissions="resourcemanager.projects.get" --stage=GA
gcloud projects add-iam-policy-binding <YOUR_PROJECT_ID> \
  --member='serviceAccount:my-iam-account@<YOUR_PROJECT_ID>.iam.gserviceaccount.com' \
  --role='projects/<YOUR_PROJECT_ID>/roles/sysdig.repositorylist'
```

### Create a Custom Role at Organization Level

```yaml

gcloud iam roles create sysdig.repositorylist --organization=<YOUR_ORGANIZATION_ID> \
    --title="Sysdig - Artifact Registry - List Repositories" \
    --description="Sysdig - Artifact Registry - List Repositories on dockerv2 _catalog API endpoint" \
    --permissions="resourcemanager.projects.get" --stage=GA
gcloud projects add-iam-policy-binding <YOUR_ORGANIZATION_ID> \
  --member='serviceAccount:my-iam-account@<YOUR_PROJECT_ID>.iam.gserviceaccount.com' \
  --role='organizations/<YOUR_ORGANIZATION_ID>/roles/sysdig.repositorylist'
```

