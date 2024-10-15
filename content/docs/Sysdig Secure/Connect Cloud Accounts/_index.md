---
linkTitle: "Connect Cloud Accounts"
title: "Connect Cloud Accounts"
weight: "1"
no_list: true
aliases:
  - /en/docs/installation/sysdig-secure-for-cloud/deploy-sysdig-secure-for-cloud-agentless/
  - /en/docs/installation/sysdig-secure-for-cloud/
  - /en/cloud-accounts-secure/
  - /en/sysdig-secure-for-cloud.html

description: "The Sysdig Secure platform for cloud accounts enables teams to secure builds, detect and respond to runtime threats, and continuously manage cloud configurations, permissions, and compliance. AWS and GCP support all the agentless cloud features from Sysdig. Azure supports agentless CSPM, CDR, and vulnerability scanning." 

---
## Cloud Features

### Agentless Compliance and Posture Management (CSPM)

Sysdig's Compliance and Posture Management for cloud accounts includes:

* **Inventory:** Search and gain visibility into resources across your cloud and Kubernetes environments. Each resource is enriched with a 360-overview of misconfigurations, compliance violations, vulnerabilities, and more. 
* **Compliance:** Review and remediate risk and compliance violations of your business zones against the policies with which you need to comply.
* **Infrastructure as Code (IaC):** This feature highlights and resolves misconfigurations and policy violations early in the development lifecycle, moving security close to the source as early as possible.

### Cloud Detection and Response (CDR)

Also known as **Threat Detection**, this includes:

* **Threat Detection For Cloud:** Sysdig analyzes Cloud platform logs for known threats.
* **Managed Threat Research:** Discover new Zero Day Attacks against your cloud. 

### Identity and Access Management (CIEM)
Sysdig's CIEM provides:

* **Least Permissive Analysis:** Sysdig analyzes CloudTrail logs and offers suggestions based on the principle of least privilege (PoLP), which involves eliminating excessive permissions from all identity entities. 
* **Identity Hygiene:** Prioritize what matters using risk labels (multi-factor authentication, inactive user, admin access) that automatically map to identity and access management violations.
* **Jira Remediation:** Assign identity-related remediations through Jira.  

### Agentless Vulnerability Scanning

**Agentless Host Scanning** provides runtime vulnerability detection in cloud accounts.

## Installation Planning

Installation Wizards take you through most of the installation scenarios for your cloud provider.

### AWS 

- **Agentless Install:** 
  - Cloud Security Posture Management (CSPM)
  - Identity and Access Management (CIEM)
  - Cloud Detection and Response (CDR)
  - Vulnerability Scanning (Technical Preview)

- **Legacy Agent-Based with CIEM:** Agent-based CDR with CIEM, plus Agentless CSPM, installed using a script

### GCP

- **Agentless Install:**  
  - Cloud Security Posture Management (CSPM)
  - Identity and Access Management (CIEM)
  - Cloud Detection and Response (CDR) (Technical Preview)
  - Vulnerability Scanning (Technical Preview)

- **Agent-Based Threat Detection:**  Agent-based Threat Detection using a script

### Azure

**Agentless Install:** 

- Cloud Security Posture Management (CSPM)
- Cloud Detection and Response (CDR) (Technical Preview)
- Vulnerability Scanning (Technical Preview)


### Onboarding Types

- **Single** onboarding scopes a single AWS account, GCP project, and Azure subscription. The target can either belong to an organization or operate independently. It is primarily recommended for feature testing before configuring the organizational setup.

- **Organizational** onboarding scopes an AWS or GCP organization, or an Azure tenant. This installation is recommended to scope all the member items within the organizational landscape.

## Quick Start

To secure a cloud account:

1. Log in to Sysdig Secure as admin and select **Integrations > Cloud Accounts** and choose **AWS**, **GCP**, or **Azure**.
3. From the relevant account page, follow the wizard prompts to connect the account.

   - **[AWS](/en/aws-secure/)**
   - **[Azure](/en/azure-secure)**
   - **[GCP](/en/gcp-secure)**
