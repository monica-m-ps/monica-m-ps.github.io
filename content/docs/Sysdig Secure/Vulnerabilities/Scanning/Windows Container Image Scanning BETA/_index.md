---
linkTitle: "Windows Container Image Scanning [BETA]"
title: "Windows Container Image Scanning [BETA]"
weight: "19"
aliases:
  - /en/windows-container-image-scanning--beta-.html
---

{{% callout type="warning" %}}

**End of Life Notice**: The Sysdig Legacy Scanning Engine will reach its End of Life (EOL) on **December 31st, 2024**. After this date, it will no longer be supported or maintained. Please upgrade to our [New Scanning Engine](/en/docs/sysdig-secure/vulnerabilities) before December 31st, 2024 to ensure continuous service and support. For assistance, contact our support team or your account representative.
{{% /callout %}}

## Overview

Sysdig provides a standalone vulnerability scanning and policy engine for Windows containers called the **Scanning Inspector**. It can be used on both Windows and Linux hosts.

{{% callout type="note" %}}

This is a standalone scanning engine. There is no centralized UI, management, or historical data.

{{% /callout %}}

### Features

-   Identify Windows container image vulnerabilities from:

    -   Windows OS CVEs

-   Windows or Linux hosts

-   Reports in JSON and PDF

-   Policy support

    -   Severity

    -   Fix available

    -   Days since fixed

### Usage

You can integrate the Windows Scanning Inspector into the continuous integration and continuous delivery (CI/CD) pipeline or deploy ad hoc during development.

#### CI/CD Pipeline

The image below shows how the Scanning Inspector fits within a development pipeline. A policy can pass or fail the workflow and provide a PDF or JSON report for each CI/CD job.

![](/image/win_scan_cicd.png)

#### Ad Hoc Scanning

Developers can run the Windows Scanning Inspector anywhere Docker can be run: a machine (Mac, Windows, or Linux), virtual machine, or cloud. It provides immediate feedback on Windows OS or .NET vulnerabilities, allowing quick mitigation of known security vulnerabilities.

## Installation

### Prerequisites

- Request a Quay secret from your Sysdig representative.

- The beta build was developed on Windows Server 2019 (build 17763). Sysdig recommends you use this build for beta evaluation.

- [Configure Docker to support Windows containers](https://docs.docker.com/desktop/faqs/windowsfaqs/#how-do-i-switch-between-windows-and-linux-containers).

### Install Scanning Inspector

1.  Use the provided secret to authenticate with Quay:

    ```
    PULL_SECRET="enter secret"
    AUTH=$(echo $PULL_SECRET | base64 --decode | jq -r '.auths."quay.io".auth'| base64 --decode)
    QUAY_USERNAME=${AUTH%:*}
    QUAY_PASSWORD=${AUTH#*:}
    docker login -u "$QUAY_USERNAME" -p "$QUAY_PASSWORD" quay.io
    ```

2.  Pull the Scanning Inspector component for Windows or Linux:

    -   **Window Host/Kernel:**
        `quay.io/sysdig/scanning-inspector-windows:latest `

    -   **Linux Host/Kernel:**
        :`quay.io/sysdig/scanning-inspector-linux:latest`

3.  Run the `--help` command to see the parameters available for the
    Scanning Inspector.

    ```yaml
    docker run --rm -v $(pwd):/outdir quay.io/sysdig/scanning-inspector-linux:latest --help
    ```

## Parameters for Scanning Inspector

The` --help` command lists the available parameters and their usage. They can be divided into those related to scanning for vulnerabilities and generating a report, and those related to creating policies.

| Flag                                | Description                                                                   | Required | Argument            | Type            |
|-------------------------------------|-------------------------------------------------------------------------------|----------|---------------------|-----------------|
| `-f string `                        | output format                                                                 | yes      | pdf or json         | Vuln scan       |
| `-i` or `-image_identifier string ` | identifier of the image                                                       | yes      | `[my_image:my_tag]` | Vuln scan       |
| `-image_type string`                | image type                                                                    | yes      | tar, daemon, pull   | Vuln scan       |
| `-o string` or `-output string `    | output file path                                                              | yes      |                     | Vuln scan       |
| `-output_format string `            | output format                                                                 | yes      | pdf or json         | Vuln scan       |
| `-fix_available`                    | policy check for fix                                                          | no       |                     | Policy creation |
| `-min_days_fix int`                 | Minimum number of days once a fix for the specific vulnerability is available | no       | default -1          | Policy Creation |
| `-min_severity string`              | Minimum severity to fail for policy evaluation                                | no       |                     | Policy creation |
| `t-string`                          | The image type                                                                | yes      | tar, daemon, pull   |                 |

## Use Cases

### Scan Remote Image and Save PDF Report

In this example, the Inspector should scan a remote image on a Linux
host and save the resulting report as a PDF to `./scanResults.pdf`

```yaml
docker run --rm -v $(pwd):/outdir quay.io/sysdig/scanning-inspector-linux:latest \
  -t pull \ # pull image from remote repo
  -i mcr.microsoft.com/windows/nanoserver:10.0.17763.1518 \ # inspect container name
  -f pdf \ # format
  -o /outdir/scanResults.pdf # output name
```

### Scan Local Image Apply Policy Conditions and Generate JSON Report

In this example, the Inspector should:

-   Scan a local image on a Windows host

-   Mount the Docker socket to access the local image. This can be done with `-v "//./pipe/docker_engine://./pipe/docker_engine"` in Windows

-   Apply a policy to specify vulnerabilities with a **minimum severity** of `high` and a **minimum number of days** after the vulnerability fix is available set to `7`.

-   If the scan does not pass, the container will have an `exit 1` error.

-   The report is in JSON

<!-- -->

```yaml
docker run --rm  -v $pwd/outdir:c:/outdir quay.io/sysdig/scanning-inspector-windows:latest 
     -t pull #Pulls a public image for evaluation. Authentication credentials are not (yet) supported.
     -i mcr.microsoft.com/windows/nanoserver:10.0.17763.1518 # local image name
     -min_severity high # Any sev high or greater CVEs will fail the image scan policy
     -min_days_fix 7 # Only fail scan if found vulnerabilities have a fix for more than 7 days
     -f json # format
     -o outdir/scanResults.json # output name

```
The code above should be run in Windows Powershell as `$(pwd)` is a Powershell expression

To scan images residing locally downloaded into Windows Docker (those visible via “docker image list”), the `-t daemon`  option needs to be used. For this to work the local socket needs to be mounted with `-v //./pipe/docker_engine://./pipe/docker_engine`

```yaml
docker run --rm  -v $pwd/outdir:c:/outdir 
     -v //./pipe/docker_engine://./pipe/docker_engine
     quay.io/sysdig/scanning-inspector-windows:latest
     -t daemon 
     -i mcr.microsoft.com/windows/nanoserver:10.0.17763.1518 
     -min_severity high  
     -min_days_fix 7 
     -f json 
     -o outdir/scanResults.json
     
```
