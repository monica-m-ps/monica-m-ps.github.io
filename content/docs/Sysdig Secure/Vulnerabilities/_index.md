---
linkTitle: "Vulnerabilities"
weight: "7"
aliases:
  - /en/vulnerabilities.html
  - /en/vuln-manage/
  - /en/image-scanning.html
title: "Vulnerability Management"
no_list: true
description: "Sysdig Secure's Vulnerabilities module pulls together the scan results from your runtime, pipeline, and container registries to provide highly accurate views of vulnerability risk, access to public exploits, and risk management. Vulnerabilities replace the Scanning module from earlier versions of Sysdig Secure."
---

## About Vulnerability Management

Sysdig Secure Vulnerabilities module addresses the top requirements for effective vulnerability management:

* **Provides highly accurate views of vulnerability risk at scale**
* **Rich details provide precision** about vulnerability risk, such as CVSS vector, score, and fix age, and insights from multiple expert feeds, such as VulnDB
* **Access to public exploits** allows you to verify  security controls and patch efficiently
* **Prioritized risk data** focused on the vulnerabilities that are tied to the packages loaded at runtime
* **Accepting risks** when the vulnerability matching does not apply for your particular deployment, or you want to postpone remediation

**Note**: At this time, the Vulnerability Management engine supports: CI/CD pipeline and runtime image scanning, policies, notifications, and reporting for runtime. Registry scanning does not yet support policies.

## Understand Vulnerability Management Stages

To create an effective vulnerability management plan, it's important to recognize the various stages of the lifecycle that need to be addressed.

![](/image/pipe_reg_run_diagram.png)

### Key Concepts

* During the build phase, software vulnerabilities may be introduced when container images are defined and assembled. Container images are, by definition, immutable. Altering the contents of an image will update the ImageID and, thus, will be considered a different image by Sysdig Secure.

* Even though unique container images (ImageIDs) cannot be modified, Sysdig can identify new vulnerabilities in running containers (for example, in Kubernetes workloads) as security feeds are continuously updated. For instance, a container image with no known vulnerabilities during its build phase might be affected by a critical vulnerability discovered ten days after being deployed into the runtime. The image remains unchanged, but new security information related to the software it contains has been found.

* Sysdig uses the same fundamental concepts to analyze the contents of an image (SBOM) and match vulnerabilities but treats images differently based on their location. Sysdig can analyze the vulnerabilities of images in a development pipeline, stored in a container image registry, or used as the template for a running container, known as runtime workloads.

### Stages: Pipeline - Registry - Runtime

#### Pipeline

Any analysis conducted before the registry phase is considered a pipeline. A clear example is CI/CD builds (Jenkins, Github, etc.), but also the execution of the *sysdig-cli-scanner* binary performed on a developer laptop or using a custom scanning script.

* Pipeline images do not have runtime context.
* The scan happens outside of the execution nodes where the agent is installed:
  * CI/CD
  * External instrumentation
  * Custom scripts or image scanning plugins
* Pipeline scans are one-off vulnerability reports; the information is a static snapshot with its corresponding execution date.
  * If you want to evaluate a newer version of the image or reevaluate the same image with newer feed information, the analysis needs to be triggered again.
* Images analyzed using the *sysdig-cli-scanner* will show up in the [Pipeline](/en/docs/sysdig-secure/vulnerabilities/findings/pipeline/#review-pipeline-scans-in-the-ui) section of the vulnerability management interface.

#### Registry

 Container Registry scanning allows you to integrate Sysdig with image registries from a range of vendors. Registry scanning provides an extra layer of defense between pipeline and runtime, where:

* Software that sits in the registry before being deployed is checked for newly discovered vulnerabilities
* Third-party software that may have been installed without going through pipeline scanning will be checked

Registry scans occur as scheduled through a cron job in the installation Helm chart once per week (Saturdays at 6:00 AM) by default.

* Batch scanning is done asynchronously and separately from the development pipeline; regardless of the time it takes to scan the batch, the pipeline is unaffected.

See [Install Registry scanner(s)](/en/install-registry-scan/).

#### Runtime

Runtime workloads are executed using a container image. Accessing the Runtime section of the Vulnerabilities menu, you will be able to see those images and their vulnerability and policy evaluation.

* Runtime workloads are located in an **execution node** and are being monitored by a Sysdig agent/node analyzer, for example a Kubernetes node that is instrumented using the Sysdig agent bundle.
* Runtime workloads will offer a **live, auto-refreshing state**. This means:
  * Workloads that are no longer running will be removed from the runtime view
  * Vulnerabilities and policies evaluations will automatically refresh without any user interaction, offering always the most up-to-date information known.
    * At least once per day
* Runtime workload have a **runtime context** associated with them,  i.e. Kubernetes cluster and namespace.
* Workloads analyzed during runtime will show up in the Runtime section of the vulnerability management interface.

See [Runtime](/en/docs/sysdig-secure/vulnerabilities/findings/runtime/).

### Asset Types: Workload, Host, and Container

The way things are scanned and how results are filtered relies on how Sysdig defines and uses the concepts of "image," "workload," "host," and "container."

#### Image

Image is not officially an asset type, but is the primary unit underlying all types of scanning.

* In Pipeline and Registry scanning, Sysdig scans images used in container-based environments.

* In Runtime, images have the context of either Kubernetes or the container host environment.

These images are strictly evaluated based on the image definition without reference to the deployed context (neither runtime nor orchestrator).

All images are extracted into a Software Bill of Materials (SBOM) format that is compatible with the CycloneDX standard. See [CycloneDX](https://cyclonedx.org) for details.

#### Workload

When scanning images that are deployed in Kubernetes environments, the scanner evaluates the image in the context of the container running in a Kubernetes workload. This context makes it contextually different from images and containers.

#### Host

A host is a full-stack Operating System (OS) running on virtual infrastructure such as Cloud IaaS or Virtual Machines. The host will also have an SBOM, without the layers that are seen in images. All the context data for a host is relative to a full-stack OS with network connectivity (OS, Host Name, IPs, etc.).

#### Container

When Sysdig scans a container (either agentlessly or with the agent) it extracts a container from a Host image via the filesystem. In container scans, only the parent host context is available, not the runtime and orchestration context.

To be scanned by Sysdig,  containers must all be compliant with the Open Container Initiative (OCI).  See [OCI](https://opencontainers.org) for details.

#### Usage

When defining a Vulnerability policy or filtering Runtime vulnerability results, you can define the asset type to use.

### Key Vulnerability Management Terminology

A cybersecurity vulnerability is a flaw in a computer system that enables an adversary to gain direct access to a network or system to compromise security and inflict harm.

#### CVE

[Common Vulnerabilities and Exposures](https://cve.mitre.org/) (CVE) is the database of such known cybersecurity vulnerabilities and exposures.

You can view the following information for all the asset types in all the vulnerability management stages:

* The severity level reported by [National Vulnerability Database](https://nvd.nist.gov/)
* The package where vulnerability was found
* The directory path to the package
* The version in which the vulnerability was fixed

#### CVSS

The [Common Vulnerability Scoring System](https://nvd.nist.gov/vuln-metrics/cvss) (CVSS) is used to assess the severity of information security vulnerabilities. Each entry in the CVE database is assigned a corresponding CVSS score, which quantifies the severity of the vulnerability.

You can view the following information for all the asset types in all the vulnerability management stages:

* The sources selected for score calculation. Fo rexample,  [National Vulnerability Database](https://nvd.nist.gov/)

* The CVSS Score and version

* The date when the vulnerability was discovered.

#### CISA KEV

The Known Exploited Vulnerabilities (KEV) Catalog maintained by CISA (Cybersecurity and infrastructure Security Agency). It is the list of software flaws that have already been exploited in the real-world.

You can view the following information for all the asset types in all the vulnerability management stages:

* The date when the vulnerability was added to the KEV catalog
* The deadline for when the vulnerability should have a fix
* If the vulnerability has been used in any Ransomware campaigns

#### Exploit

An exploit is a software program created to identify and exploit security flaws or vulnerabilities present in a system.

You can view the following information for all the asset types in all the vulnerability management stages:

* Link to the existing exploit PoC or source code
* The date when the exploit was disclosed.

#### Vulnerability Databases

Sysdig Secure continuously checks against a wide range of vulnerability databases, updating the Runtime scan results with any newly detected CVEs.

The current database list includes:

* [NIST NVD](https://nvd.nist.gov/)
* [VulnDB](https://vulndb.cyberriskanalytics.com/users/sign_in)
* [NPM](https://www.npmjs.com/advisories)
* [Python](https://www.python.org/dev/security/)
* [Ruby](https://www.ruby-lang.org/en/security/)
* [Alpine Linux](https://security.alpinelinux.org/)
* [Centos](https://access.redhat.com/security/)
* [Debian](https://security-tracker.debian.org/tracker/data/json)
* [Red Hat](https://www.redhat.com/security/data/oval/v2/feed.json)
* [Rocky ERRATA](https://errata.rockylinux.org/)
* [Ubuntu](https://ubuntu.com/security/cves)
* [Amazon Linux](https://alas.aws.amazon.com/)
* [Alibaba Linux](https://www.alibabacloud.com/help/en/alinux/product-overview/security-bulletin)
* [Oracle Linux](https://linux.oracle.com/security/)
* [Chainguard](https://images.chainguard.dev/security)
* [Wolfi](https://github.com/wolfi-dev/os/security)

## Supported Operating Systems

The Vulnerability Management scanners can scan any OS, but for the following they will also report vendor-specific information.

* Alibaba Linux
* Alpine
* Amazon Linux 2022/2023
* CentOS 7
* CentOS 8-stream
* CentOS 9-stream
* Debian
* RHEL

  The Sysdig Secure UI displays [Red Hat Security Updates](https://access.redhat.com/security/security-updates/security-advisories) in the UI and in CLI version > 1.6.1 using the option `--output-json`. Note that this information is not included in Reports.

* Ubuntu
* Ubuntu kinetic v22.10
* Ubuntu Lunar v23.04

### Runtime

* Kubernetes Runtime and Hosts
* Supported container runtimes:
  * Docker daemon
  * ContainerD
  * CRI-O

### Agentless Host Scanning

**Cloud Hosts** (AWS EC2): Discovery of running hosts

#### Supported Operating Systems

* Debian 10 and Debian 11
* Ubuntu 20.04 LTS and Ubuntu 22.04 LTS
* Amazon Linux 2 and Amazon Linux 2023
* RedHat 8 and RedHat 9
* Operating Systems that the agent supports

### CI/CD Pipeline

#### Supported Container Image Formats

* Docker Registry V2 - compatible
* Docker Daemon
* Podman
* Docker Archive (tar)
* OCI Archive

#### Supported Package Types

* Debian
* Alpine
* RHEL: The Sysdig Secure UI displays [Red Hat Security Updates](https://access.redhat.com/security/security-updates/security-advisories) in the UI and in CLI version > 1.6.1 using the option `--output-json`. Note that this information is not included in Reports.

* Ubuntu
* Java JAR, WAR, EAR
* Golang (built with go 1.13+)
* Pypi (Python)
* NPM (JS)
* Ruby Gems
* NuGet (.Net)
* Cargo (Rust)
* Composer (PHP)

#### Supported Container Image CPU Architectures

* linux/amd64
* linux/arm64
