---
linkTitle: "Install Container Registry Scanner"
title: "Install Registry Scanner"
weight: "13"
no_list: true
aliases:
  - /en/docs/installation/container-registry-scanner/
  - /en/install-registry-scan/
  - /en/docs/installation/sysdig-secure/install-registry-scanner/
description: Integrate Sysdig Secure with your container registry to add a layer of defense between Pipeline and Runtime and to enhance defense depth. This page describes how to install and configure the registry scanner on various private registries. See [Vulnerabilities | Registry](/en/vuln-registry-scan/) to review the scan results in the UI.
lab_url: https://enablement.sysdig.com/registry-image-scanning-with-sysdig-secure
---

## Setup

### Prerequisites

- Helm v3.8 or above
- A **Kubernetes cluster** managed with **Helm**

  The registry scanner is installed on this cluster
- A [Sysdig Secure API token](/en/api-token/)

- Recommended: Set up a [service account](/en/docs/administration/administration-settings/user-and-team-administration/manage-teams-and-roles/#service-accounts) with minimal privileges, such as a [**Custom** role](/en/docs/administration/administration-settings/access-and-secrets/user-and-team-administration/manage-custom-roles/)
    - The **Custom** role will require the Vulnerability Management permissions for Sysdig Secure:
        - **Registry Scanner** `exec` 
        - **Scan Results**: `read`
       

- Network access to the target registry for the installed components
- Outbound network access to the Sysdig backend to store results

  If you are behind a firewall, follow the [outbound network rules for vulnerability scanning](/en/docs/administration/saas-regions-and-ip-ranges/#secure-vulnerability-management-and-scanning).

- The Registry Scanner node must possess the capacity to execute a Docker pull from the node. Use the following command to confirm this:

   ```
    docker pull ${THE_REGISTRY}/${A_IMAGE}
    ```

### Supported Vendors

- [AWS Elastic Container Registry](/en/aws-eleastic-container-registry) (ECR)
   Single Registry, Organizational
- [JFrog Artifactory](/en/identify-jfrog-artifactory-configuration)<br/> SaaS, OnPrem
- [Azure Container Registry](/en/azure-container-registry/) (ACR)<br/> Single Registry
- [IBM Container Registry](/en/ibm-container-registry-scan/) (ICR)
- [Quay.io](/en/quay-registry-scan/) <br>SaaS
- [Harbor](/en/harbor/)
- [Google Artifact Registry](/en/google-registry) (GAR), [Container Registry](/en/google-registry) (GCR)<br/> Single Registry
- [Nexus](/en/nexus-scan/)
- [OpenShift Container Platform Registry](/en/ocp-registry-scan/)

### Known Limitations

- A new registry scanner must be installed per registry (except for AWS Organization).
- Public registries are not supported.
- Images that have been scanned and reported to Sysdig are rescanned only on the designated refresh cycle. Scans are refreshed on a scheduled Helm cron job, every Saturday at 6:00 AM by default. You can adjust the schedule.
- Check vendor-specific limitations on the relevant subpage.
- Registry Scanner does not support multi-architecture images.

## Installation

{{% callout type="warning" %}}

**Legacy scanning engine:** If you still use the legacy scanning engine and want to keep running that version, pin the Helm chart version to *0.1.39*. If you deploy a later version, the current vulnerability management engine will replace the legacy installation.

{{% /callout %}}

### Overview

1. Install the [Registry Scanner](https://charts.sysdig.com/charts/registry-scanner/) Helm Chart:

   ```
   helm repo add sysdig https://charts.sysdig.com
   helm repo update
   ```

2. Prepare the Helm chart for your specific registry vendor.
   Provide:
   - `<SYSDIG_SECURE_URL>`: Region-dependent [Sysdig Secure endpoint](/en/saas-regions/)
   - `<SYSDIG_SECURE_API_TOKEN>`: See [Retrieve the Sysdig API Token](/en/api-token/)<br/><br/>

3. Launch Helm instructions and allow some time for the first scan.

4. Check results in the Sysdig Secure [Vulnerabilities | Registry](/en/vuln-registry-scan/) UI.

### Upgrade

Perform regular helm chart upgrade procedure:

1. Upgrade the repository with `helm repo update`
2. Re-launch the `helm upgrade --install` with the values from the previous installation.


## Next Steps

[Review scan results](/en/vuln-registry-scan/) in the Sysdig Secure UI.

[Create scanning reports](/en/reporting-secure/)
