---
linkTitle: "Integrate with Container Registries"
title: "Integrate with Container Registries"
weight: "11"
aliases:
  - /en/integrate-with-container-registries.html
---

{{% callout type="warning" %}}

**End of Life Notice**: The Sysdig Legacy Scanning Engine will reach its End of Life (EOL) on **December 31st, 2024**. After this date, it will no longer be supported or maintained. Please upgrade to our [New Scanning Engine](/en/docs/sysdig-secure/vulnerabilities) before December 31st, 2024 to ensure continuous service and support. For assistance, contact our support team or your account representative.
{{% /callout %}}

Some image registries can trigger an automatic event or action every
time a new container is pushed into the registry. Sysdig has provided
seamless integration with AWS Elastic Container Registries (ECR) for
this kind of automatic scanning. See [ECR Registry
Scanning](/en/docs/sysdig-secure/vulnerabilities/scanning/integrate-with-container-registries/#ecr-registry-scanning).

Other image scanning pipelines use the container registry as a "passive"
element; when you know which image you want to scan, you retrieve the
target image from the registry. For this type of integration, you must
provide the registry credentials in the Sysdig Secure interface. See
[Manage Registry
Credentials](/en/docs/sysdig-secure/vulnerabilities/scanning/integrate-with-container-registries/#manage-registry-credentials),
below.

{{% callout type="tip" %}}

Review the [Types of Secure
Integrations](/en/docs/sysdig-secure/integrations-for-sysdig-secure/#integrations-for-sysdig-secure) table for more
context. The Container Registries column lists the various options and
their levels of support.

{{% /callout %}}

## ECR Registry Scanning

ECR Registry Scanning automatically scans all container images pushed to
all your Elastic Container Registries, so you have a vulnerability
report available in your Sysdig Secure dashboard at all times, without
having to set up any additional pipeline.

An ephemeral CodeBuild pipeline is created each time a new image is
pushed, which executes an inline scan based on your defined scan
policies. Default policies cover vulnerabilities and dockerfile best
practices, and you can define advanced rules yourself.

### Usage Steps

1. **Deploy:** [Deploy Sysdig Secure for cloud on
    AWS](/en/cloud-accounts-secure/) and choose the
    `ECR Image Registry Scanning` option.

2. **[Insights](/en/docs/sysdig-secure/insights/#insights)** becomes
    your default landing page in Sysdig Secure.

3. **Review the Scan Results** to confirm that *AWS Registry* appears
    and use the feature.

    ![](/image/scan_aws.jpg)

## Manage Registry Credentials

Registry credentials are required for Sysdig Secure to pull and analyze
images. Each of the registry types has unique input fields for the
credentials required (e.g., `username/password` for docker.io;
`JSON key` for Google Container Registry).

The login requires at least `read` permissions.

### Add a New Registry

1. From the `Image Scanning` module, select `Registry Credentials` and
    click `Add Registry`.

    The New Registry page is displayed.

    ![](/image/reg_cred.png)

2. Enter the basic identification information:

    `Registry Name:` Self-defined

    `Path` The path to the registry, e.g. `docker.io`

    **NOTE:** In some cases, you may have a registry with various
    namespaces, each with different permissions.

    **You can use a partial path name with a wildcard (\*) to subsume
    all those credentials into the single set of credentials you
    configure on this page.**

    The wildcard indicates that any image located under the partial path
    inside the registry (`/rg-2-1er` in the screenshot) will use the
    registry credentials configured in step 3, below.

    `Type` Select from the from the drop-down menu.

3. Configure the registry-specific `credentials` (based on the `Type`
    chosen).

    **Docker V2** There are many Docker V2 registries, and the
    credential requirements may differ.

    For **Azure Container Registry:**

    1. Admin Account

        `Username`: in the
        `'az acr credentials show --name <registry name>'` command
        result

        `Password`: The password or password2 value from the
        `'az acr credentials show'` command result

    2. Service Principal

        `Username`: The service principal app id

        `Password`: The service principal password

    **Google Container Registry:**

    JSON Key

4. **(Primarily for OpenShift clusters):** Add an
    `internal registry address`.

    The recommended way to run an image registry for an OpenShift
    cluster is to run it locally. The Sysdig agent will detect the
    internal registry names, but for the Anchore engine to pull and scan
    the image it needs access to the internal registry itself.

    **Example:**

    External name: `mytestregistry.example.com`

    **Internal name:** `docker-registry.default.svc:5000`

{{% callout type="note" %}}

Sysdig maps the internal registry name to the external registry
name, so the [`Runtime` and `Repository`
lists](/en/docs/sysdig-secure/vulnerabilities/scanning/review-scan-results/#review-scan-results) will show only
the external names.

{{% /callout %}}

5. **Optional:** Toggle the switch to `Allow Self-Signed` certificates.

    By default, the UI will only pull images from a TLS/SSL-enabled
    registry.

    Toggle `Allow Self-Signed` to instruct the UI **not** to validate
    the certificate (if the registry is protected with a self-signed
    certificate or a cert from an unknown certificate authority).

6. **Optional:** Toggle the `Test Credentials`switch to validate your
    entries.

    When enabled, Sysdig will attempt to pull the image using the
    entered credentials. If it succeeds, the registry will be saved. If
    it fails, you will receive an error and can correct the credentials
    or image details.

    If enabled, then enter the `test registry path` in the format :

    ```yaml
    registry/repo:tag
    ```

    E.g. **`quay.io/sysdig/agent:0.89`**

7. Click `Save`.

### Edit a Registry

1. From the `Image Scanning` module, select `Registry Credentials`.

2. Select an existing registry to open the `Edit` window.

3. Update the parameters as necessary and click `Save`.

{{% callout type="note" %}}

    The registry Type cannot be edited.

{{% /callout %}}

### Delete a Registry

1. From the `Image Scanning` module, select `Registry Credentials`.

2. Select the existing registry to open the `Edit` window.

3. Click `Delete Registry`and click `Yes` to confirm the change.

### Next Steps

When at least one registry has been added successfully, it is possible
to scan images and [review scan
results](/en/docs/sysdig-secure/vulnerabilities/scanning/review-scan-results/#review-scan-results), taking advantage
of the `Default` scanning policy provided.
