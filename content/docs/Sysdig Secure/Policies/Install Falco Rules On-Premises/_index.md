---
linkTitle: "Install Falco Rules On-Premises (Legacy)"
title: "Install Falco Rules On-Premises (Legacy)"
weight: "24"
aliases:
  - /en/install-falco-rules-on-premises.html
description: "Periodically, Sysdig releases new [Falco Rules](/en/docs/release-notes/falco-rules-changelog) that provide additional coverage for new behaviors, and adds exceptions for known good behaviors. The rules installer is included by default in on-prem installations 6.x and up. If you are using an earlier version, you can install Falco Rules as a container in on-prem deployments."
---

For air-gapped deployments, the instructions vary slightly. See [Airgapped Environments](#airgapped-environment).

## Download the Container Image

Sysdig provides a container image on Quay.io to install Falco Rules on the Sysdig Platform. See [Falco Rules Installer](https://quay.io/repository/sysdig/falco-rules-installer).

This container image allows easy installation and upgrades of the Falco rules files for Sysdig Secure. The file contains the following:

-   The rule files.

-   The latest version of Falco.

-   The [sysdig-sdk-python](https://github.com/sysdiglabs/sysdig-sdk-python) wrappers that deploy the rule files to a Sysdig platform deployment.

The image is tagged with new versions as new sets of rules files are released, and the `latest` tag always points to the latest version.

When a container is run with this image, it does the following:

-   Validates the rules.

-   Fetches the custom rules file and verifies compatibility with the to-be-deployed default Falco rules file.

-   Deploys the rules to the configured Sysdig Platform backend component.

{{% callout type="note" %}}

The Falco Rules Updater can be run from ANY machine on the same network
as the backend that has Docker installed. It does not have to be the
backend server.

{{% /callout %}}

## Install Falco Rules

### Prerequisites

- On-prem version 5.x or earlier

Starting from 6.x, the rules installer is included in on-premises installations by default. It checks for rules updates daily and installs the rules automatically, unless disabled.

### Non-Airgapped Environment

If the installation machine has network access to pull the image from the Docker hub:

1.  Download the container image:

    ```yaml
    docker pull quay.io/sysdig/falco-rules-installer:latest
    ```

2.  Use the `docker run` to install the Falco Rules. For example:

    ```yaml
    docker run --rm --name falco-rules-installer --network host -it -e DEPLOY_HOSTNAME=https://my-sysdig-backend.com -e DEPLOY_USER_NAME=test@sysdig.com -e DEPLOY_USER_PASSWORD=<my password> -e VALIDATE_RULES=yes -e DEPLOY_RULES=yes -e CREATE_NEW_POLICIES=no -e SDC_SSL_VERIFY=True -e ENV_TYPE=onprem quay.io/sysdig/falco-rules-installer:latest
    ```

### Airgapped Environment

If the installation machine does not have the network access to pull the image from the Docker hub:

1.  Download the container image on a machine that is connected to the network:

    ```yaml
    docker pull quay.io/sysdig/falco-rules-installer:latest
    ```

2.  Create an archive file for the image:

    ```yaml
    docker save quay.io/sysdig/falco-rules-installer:latest -o falco-rules-installer.tar
    ```

3.  Transfer the tar file to the air-gapped machine.

4.  Untar the image file:

    ```yaml
    docker load -i falco-rules-installer.tar
    ```

    It restores both images and tags.

5.  Use the `docker run` to install the Falco Rules. For example:

    ```yaml
    docker run --rm --name falco-rules-installer --network host -it -e DEPLOY_HOSTNAME=https://my-sysdig-backend.com -e DEPLOY_USER_NAME=test@sysdig.com -e DEPLOY_USER_PASSWORD=<my password> -e VALIDATE_RULES=yes -e DEPLOY_RULES=yes -e CREATE_NEW_POLICIES=no -e SDC_SSL_VERIFY=True -e ENV_TYPE=onprem sysdig/falco_rules_installer:latest
    ```

## Use Falco Rules

You can run this container from any host with access to the server that hosts the Sysdig backend API endpoint. The hostname is specified in the `DEPLOY_HOSTNAME` variable. The container doesn't need to run on the hosts where the Sysdig Platform backend components are running.

The container depends on the following environment variables to run:

| Variables     | Description        |
| ---------------------- |-------------------|
| `DEPLOY_HOSTNAME`      | The server that hosts the Sysdig API endpoints. The default is <https://secure.sysdig.com>.      |
| `ENV_TYPE` | The environment deploying to.  Set to `onprem` unless deploying to SaaS.  |                                  
| `DEPLOY_USER_NAME`     | The username for the account that has the admin-level access to the Sysdig API endpoints. The value defaults to a meaningless user, `nobody@nobody.com`.     |
| `DEPLOY_USER_PASSWORD` | The password for the admin user. The value defaults to a meaningless password `nopassword`.   |
| `VALIDATE_RULES`       | If set to yes, ensure that the rules file is compatible with your user rules file. Otherwise, skip this validation step. The value defaults to `yes`.  |
| `DEPLOY_RULES`         | If set to yes, the falco rules file is deployed. Otherwise, skip deploying the falco rules file. The value defaults to `yes`.    |
| `CREATE_NEW_POLICIES`  | If set to yes, will fetch new DEFAULT runtime policies, and restore any missing/deleted DEFAULT runtime policies. This will NOT overwrite any of your existing runtime policies.  The value default is `no`.          |
| `SDC_SSL_VERIFY`       | If set to false, allow certificate validation failures when deploying the rules. The value defaults to `true`.                |
| `SKIP_FALCO_VERSION_0` | If set to `yes`, will not deploy falco rules file version 0, only deploys version 8. (Recommended for on-prem customers with [version 5.x]. Default value is `yes`.                                                                       |
| `SKIP_K8_VERSION_2`    | If set to `yes`, will not deploy k8 audit rules file version 2, only deploy version 8 (Recommended for on-prem customers with  [version 5.x]. Default value is `yes`.                                                                      |
| `BEARER_TOKEN` | If set to the value of the [Secure API token](/en/docs/administration/administration-settings/user-profile-and-password/retrieve-the-sysdig-api-token/) for an admin account, will skip logging in and retrieving token through API calls. If not set, will use username and password to retrieve token. Default value is 'notset'. |

## Learn More

For the latest information about the image and usage, see [Falco Rules Installer on Quay.io](https://quay.io/repository/sysdig/falco-rules-installer)