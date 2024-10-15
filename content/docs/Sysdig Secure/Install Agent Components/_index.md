---
linkTitle: "Installation"
title: "Installation"
weight: "2"
no_list: true
aliases:
  - /en/docs/installation/sysdig-agent/agent-installation/agent-installation-requirements/
  - /en/install-secure
  - /en/installation.html
  - /en/installation
  - /en/agent-installation
  - /en/agent-installation.html
  - /en/docs/installation
  - /en/docs/installation/sysdig-agent/agent-installation/
  - /en/docs/installation/sysdig-agent/
  - /en/docs/installation/sysdig-agent/agent-installation/quick-install-sysdig-agent-/
description: "To use Sysdig Secure alone, or Sysdig Secure and Sysdig Monitor together, follow the installation instructions in this section. Review the features and components and choose the installation path that fits your environment."
---

Sysdig Secure provides container, Kubernetes, and cloud security for the entire enterprise, from pipeline development through incident response. 

![](/image/secure_website_image.png)

The following section describes Sysdig Secure features and the tools that provide them.

## Runtime Threat Detection

Runtime threat detection is provided by the Sysdig agent, which processes syscall events and metrics, creates capture files, and performs auditing and compliance. It provides detailed visibility into container and host activity, helping to detect and prevent threats.

There is also a serverless agent available for ECS Fargate. 

**Install the Sysdig Agent** on **[Kubernetes](/en/install-kubernetes-secure/) \| [Hosts](/en/install-hosts-secure/) \| [ECS on EC2](/en/install/ecs-ec2-secure/)**

**Install the Serverless Agent** on **[ECS Fargate](/en/install-ecs-fargate-secure/)**

## Vulnerability Management

Vulnerability management is the systematic process of identifying, evaluating, and addressing security-related software bugs in your organization, as identified by trusted third-party vulnerability feeds. Key concepts areas of VM include vulnerability identification, risk assessment and prioritization. Sysdig addresses vuln findings at each stage of the software lifecycle. 

### Vulnerability Pipeline Scanning 

The `sysdig-cli-scanner` tool allows you to scan a container image stored locally (such as a developer's machine) or in a remote registry. You can also integrate the `sysdig-cli-scanner` as part of any instrumentation or CI/CD pipeline to scan any container image right after it is built. Native plugins for some CI/CD software, such as Jenkins, are also available directly from their marketplace.

**[Install as a binary on your pipeline](/en/install-vuln-cli-scan/)**

### Registry Scanning

This registry scanning component deploys the Sysdig Registry Scanner as a scheduled cron job in your Kubernetes cluster. It scans container images stored in the registry for vulnerabilities and compliance issues before they are deployed.

**[Install the Registry Scanner](/en/install-registry-scan/)** for a range of registry vendors. 

### Runtime Scanning

Runtime scanning includes both Kubernetes workloads and hosts. 

Sysdig's runtime scanner automatically observes and reports on all the runtime workloads in containers, keeping a close-to-real time view of images and workloads executing on the different Kubernetes scopes of your infrastructure. Perform periodic re-scans, guaranteeing that the vulnerabilities associated with the Runtime workloads and images are up-to-date with the latest vulnerabilities feed databases. It will automatically match a newly reported vulnerability to your runtime workloads without requiring any additional user interaction.

**Installed with the Sysdig Agent on [Kubernetes](/en/install-kubernetes-secure/) |[Hosts](/en/install-hosts-secure/)**

### Vulnerability Host Scanning

The host scanning component for vulnerabilities analyzes the software packages installed on hosts and shows the scan results in the Runtime view page. Perform periodic re-scans, guaranteeing that the vulnerabilities associated with the software packages installed are up-to-date with the latest vulnerabilities feed databases. It will automatically match a newly reported vulnerability to your hosts without requiring any additional user interaction.

**Installed with the Sysdig Agent on [Kubernetes](/en/install-kubernetes-secure/) or directly on a [host as container](/en/install-container-host-scanner) or  [host as package](/en/install-package-host-scanner)**

OR

**Installed [agentlessly](/en/aws-agentless) for AWS cloud accounts** 

In addition:

While runtime scanning scans containers in Kubernetes environments, host scanning can be extended to scan non-Kubernetes containers. 

**Extend [Host Scanning for non-Kubernetes containers](/en/nonk8s-container-scan)**

## Compliance 

### Kubernetes (KSPM)

To scan for compliance violations in Kubernetes environments: 

#### KSPM Analyzer

This Kubernetes Security Posture Management component analyzes your host's configuration and sends the output to be evaluated against compliance policies. The scan results are displayed in Sysdig Secure's [Compliance](/en/compliance/) UI.
**Install on [Kubernetes](/en/install-kubernetes-secure/)**

#### KSPM Collector

This component enables the collection and sending of Kubernetes resource manifests to be evaluated against [Compliance policies](/en/posture-policies/).  The scan results are displayed in Sysdig Secure's [Compliance](/en/compliance/) UI.

**Install on [Kubernetes](/en/install-kubernetes-secure/)**

### Containers (Non-Kubernetes)

#### Posture Host Analyzer 

For hosts running Docker without a Kubernetes orchestrator, to scan, evaluate against  [Compliance policies](/en/posture-policies/), and display scan results in Sysdig Secure's [Compliance](/en/compliance/) UI.

**Install on [Linux Host running Docker]( /en/install-container-posture-host-analyzer)**

## Response

### Rapid Response

This component allows designated advanced users to investigate and troubleshoot events from a remote shell. This feature helps in quick incident response and resolution.

**Install on [Kubernetes](/en/install-rapid-response-k8s/)** or on a **[host as a container](/en/install-container-rapid-response/)**

## Admission Controller

### Kubernetes Audit Logging

This component deploys the Sysdig Admission Controller in your Kubernetes cluster to enable  audit logging. It helps in enforcing security policies and preventing the deployment of vulnerable images.

**[Install on Kubernetes](/en/install-audit-logging)**



## Cloud Account Features

Integrate Sysdig Secure features to your cloud environments to provide unified threat detection, compliance, forensics, and analysis. The Agentless CSPM and Threat Detection features are available on AWS, Azure, and GCP. CIEM (Identity and Access) is currently available on AWS.

**[Connect Cloud Accounts](/en/cloud-accounts-secure)** 

### Agentless CSPM 

Includes: 

* **[Inventory](/en/inventory/)**: Search and gain visibility into resources across your cloud (GCP, Azure, and AWS) and Kubernetes environments. Each resource is enriched with a 360 overview - misconfigurations, compliance violations, vulnerabilities, and more. 
* **[Compliance](/en/compliance/):**: Review and remediate risk and compliance violations of your business [zones](/en/zones/) against the [policies](//en/posture-policies/) with which you need to comply.
* **[IAC](/en/iac-landing/)**: Highlights and resolves misconfigurations and policy violations early in the development lifecycle, moving security as close to source as early as possible.

### Threat Detection for Cloud

Includes:

* **[Threat Detection](/en/threat-detect/) For Cloud**: Analyzing Cloud platform logs for known threats. 
* **[Managed Threat Research](/en/threat-policies/)**: Discover new Zero Day Attacks against your cloud. 

### Identity and Access (CIEM)

[**Available for AWS**](/en/ia-ciem/) and includes: 

* **Least Permissive Analysis:** By analyzing CloudTrail logs, we offer suggestions following the principle of least privilege (PoLP) - eliminating excessive permissions from all identity entities. 
* **Identity Hygiene:** Prioritize what matters using risk labels (multi-factor authentication, inactive user, admin access) that automatically map to identity and access management violations.
* **JIRA Remediation:** Assign identity-related remediations through JIRA.  

Go to **[Connect Cloud Accounts](/en/cloud-accounts-secure)**  to choose the cloud provider and cloud components for your environment. 

## Get Started

* **[Install Sysdig Agent and components](/en/install-components-secure)**, based on your environment
* **[Connect Cloud Accounts](/en/cloud-accounts-secure)**
* **Connect peripherals** such as: 
  * [Sysdig Vulnerability CLI Scanner](/en/install-vuln-cli-scan)
  * [Sysdig Registry Scanner](/en/install-registry-scan)
  * [Rapid Response](/en/rapid-response.html)
  
* See also **[Installation Requirements](/en/install-requirements-secure)** 

## Warranty Disclaimer

Customer understands and agrees that it is impossible under any current available technology for any security software to identify one hundred percent (100%) of cloud threats, vulnerabilities, malicious software or attacker’s behavior. Sysdig Secure relies upon threat feeds, behavioral analysis, machine learning, and other techniques, but these may not be enough to discover all attacks. Additionally, Customer understands and agrees that Sysdig Secure may incorrectly identify cloud threats, vulnerabilities, potentially malicious software or attacker’s behavior as a potential threat (“False Positive”). SYSDIG DOES NOT GUARANTEE OR WARRANT THAT IT WILL FIND, LOCATE, OR DISCOVER ALL THREATS OR THAT ALL THREATS IT SURFACES ARE FREE FROM FALSE POSITIVES, AND IN USING SYSDIG SECURE CUSTOMER ASSUMES ALL RISK AND LIABILITY.
