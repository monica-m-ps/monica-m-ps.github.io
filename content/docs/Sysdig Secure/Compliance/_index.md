---
linkTitle: "Compliance"
title: "Compliance"
weight: "8"
aliases:
  - /en/action-comp.html
  - /en/compliance.html
  - /en/docs/sysdig-secure/posture/compliance/actionable-compliance/
  - /en/benchmarks.html
  - /en/compliance/
  - /en/docs/sysdig-secure/posture/compliance/
  - /en/posture.html
no_list: true
description: "The Compliance module checks selected controls from various compliance standards, and compiles and reports the findings. It evaluates Sysdig image scanning policies, Threat Detection runtime policies and rules, scheduled benchmark testing, and Admission Controller to provide you with the outcomes. New standards are being added regularly, and additional compliance coverage will be introduced over time. The checks include compliance handling for both Kubernetes and Cloud accounts (KSPM/CSPM), as well as identity and access for cloud accounts. Note that Sysdig cannot check all controls within a framework, such as those related to physical security." 
---

Access **Compliance** from the left navigation menu.

{{< callout type="info" >}}

Compliance is not available for Managed Falco (Secure light).

{{% /callout %}}

## Benefits

- Compliance that is actionable:
  - Compliance lets you manage your risks if you have the required permissions. Take action to:
    - Remediate

    - Accept the risk

    - Open a Pull Request in your code repository.

      Applicable only of if Git Infrastructure as Code (IaC) integration is enabled
  
- Collected Violations:
  - The resources defined by your [Zones](/en/zones/) are evaluated against compliance policies.
  
  - Violations are collected into tiles and shown on the Compliance page.

  - Every day, resources are sent to the backend, where Sysdig performs relevant analysis of policies.

  - You can create custom policies or use Sysdig out-of-the-box policies.

- Intuitive user interface (UI): Click the resource itself, rather than navigate a list of violations.
  
- Download reports, supported by APIs.

## Use Cases

### Compliance and Security Team Members

Compliance and Security Team Members might want to:

- Check the current compliance status of their business zones against predefined policies
- Demonstrate to an auditor the compliance status of their business zone in a specific point in time (the audit)
- Create a report of the compliance status of their business zone, and share it with their auditors and the management team
- Understand the magnitude of the compliance gap

### DevOps Team Members

DevOps Team Members might want to:

- Identify the compliance violations of a predefined policy  applied on their business zones
- Manage the violations according to their severity
- Easily fix the violation
- Document exceptions and accept risk according to the risk management policy of their organization

## Prerequisites

To populate the **Compliance** module with data, ensure that you have prepared your environment to connect to Sysdig Secure:

- Connect [Cloud Accounts](/en/cloud-accounts-secure/)
- [Install Sysdig Agent](/en/install-kubernetes-secure/) in your Kubernetes environment
  - Include `--set global.kspm.deploy=true \` while installing Sysdig Agent.
  - If you are running Kubernetes and want to identify which version you have, see [KSPM](/en/docs/installation/kspm-components/).

## Understand the Compliance UI

On the **Compliance** page, you can review the compliance posture for each of your zones. Each row shows the compliance findings of a policy that is applied to your zone.

Filter the list with the **Select Zones** and **Select Policies** dropdowns.

![](/image/compliance.png)

The Compliance table is made up of the following columns:

- **Zone / Policy:** This is the lens to evaluate your compliance results through your zones and the policies you applied to them.

- **Passing Score:** The number of requirements passing for this policy view, expressed as a percent. The percent of resources passing (or accepted) out of all resources evaluated. Resources are the most granular of your results. The higher the percentage, the fewer individual resources failing, the better. The higher the better.

- **Requirements Failing:** The number of requirements remaining to fix to get to 100% for a view, listed as a bar chart of the past 7 days' results. The smaller the number, the better. Requirements are made up of one or more controls, so requirements will be the smaller number.

- **Controls to Fix:** The number of controls to fix to achieve a perfect score. The smaller the better. (Multiple controls make up a single requirement, so the control count will be larger than the requirement count).

- **Resource Violations by Severity:** The number of resources failing, organized by severity. The severity can be High, Medium, or Low. One resource can be counted multiple times if it’s failing multiple controls. The fewer, the better.

- **Accepted Risks**: The number of violations you have chosen to accept the risk for. Risks can be accepted at the level of an individual resource, or globally on a control for all resources that match a given zone.

     For more information, see [Accepted Risk](/en/risk).

### Favorites

Select or deselect the star beside any policy or zone view to add it to your favorites.

Select **My Favorites** to filter the policy list by Favorites.

Favorites are displayed on the **Home** page.

## Detect and Remediate Vulnerabilities

To detect prioritized vulnerabilities, analyze them, and remediate them in Compliance, follow these steps:

1. On the **Compliance** page, review high-level posture performance indicators (PPIs) on each of the policies applied on your zones.

2. Select a Policy to see its **Findings** and select a failing requirement to see the Controls and failing resources that comprise it.

3. Select **View Remediation** to open the Remediation panel.

    ![](/image/view-remediation-control.png)

4. On the Remediation panel, you can **Review Issues** where possible, and consult **Remediation Guidelines** for possible fixes. You can remediate:

    - Manually: Copy the code and apply it in production.

    - Open a Pull Request: If you have a Git Integration, choose the relevant Git source and Compliance will create a pull request integrating the fix (as well as checking for code formatting cleanup). You can review all the changes in the PR before you merge.

5. Optionally, **Accept the Risk** and remove the violation from the failed controls. When accepting the risk you can leave a note as to the reason, and choose an expiration period for the acceptance. Risk can be accepted at the level of an individual resource, or globally on a control for all resources that match a given zone.

6. Optionally, select **Download Report** for a .CSV spreadsheet of your compliance results for development teams, executives, or auditors.

## CSPM Zones Management

On the Compliance page, a default **Entire Infrastructure** zone is automatically created. Center for Internet Security (CIS) policies and the Sysdig Kubernetes policy are automatically added to the **Entire Infrastructure** zone.

To see results from any of the dozens of out-of-the-box policies provided with the Compliance module, or for any custom policies, you must apply them to a zone.

Select **Policies** > **Posture** | **Policies** to create, edit, and apply zones to policies.

### Use the CSPM API

When your organization uses a third-party system to receive remediation reports and create tasks, consider using the CSPM APIs.

These are documented online along with the rest of the Sysdig Secure APIs.

For API doc links for additional regions or steps to access them from within the Sysdig Secure UI, see the [Developer Tools](/en/docs/developer-tools/) overview.

##### Compliance Results API Call Requirements

- Please specify a zone in the request. If a zone is not specified in the request, results will be returned for policies applied on the default “Entire Infrastructure” zone.
- If no policy is applied on the default “Entire Infrastructure” zone, you will receive empty results.
- Note that URL Links to every Control Resource List API call are contained in the Compliance Results Response.

## Terminology and Policies

### Terminology Changes

| Previous Term           | New Term                                                     |
| ----------------------- | ------------------------------------------------------------ |
| Framework, Benchmark    | **Policy**<br /> The *policy* is a group of business/security/compliance/operations *requirements* that can represent a compliance standard (e.g. PCI 3.2.1), a benchmark (e.g. CIS Kubernetes 1.5.1), or a business policy (e.g. ACME corp policy v1). <br /><br />You can review the available policies and create custom CSPM/Posture policies under *Policies* |
| Scopes                  | **Zone**<br />A business group of resources for a specific customer, defined by a collection of Scopes of various resource types, calculated by “OR” operators |
| Control                 | **Requirement** (or Policy Requirement)<br /> A *requirement* exists in a single policy and is an integral part of the policy. The requirement represents a section in a policy with which compliance officers & auditors are familiar. |
| Family                  | **Requirements Group**<br /> Group of requirements in a policy |
| Rule                    | **Control**<br />A *control* defines the way we identify the issue (check) and the playbook(s) to remediate the violation detected. |
| Vulnerability Exception | **Risk Acceptance**<br /> You can review a violation or vulnerability, but not remediate it, and acknowledge it without making it fail the policy. |

## Posture Policies

The following posture policies are included out of the box:

- National Institute of Standards and Technology (NIST)
  - NIST Cybersecurity Framework (CSF) v2.0
  - NIST Privacy Framework v1.0
  - NIST SP 800-53 Rev 5
  - NIST SP 800-53 Rev 5 Privacy Baseline
  - NIST SP 800-53 Rev 5 Low Baseline
  - NIST SP 800-53 Rev 5 Moderate Baseline
  - NIST SP 800-53 Rev 5 High Baseline
  - NIST SP 800-82 Rev 2
  - NIST SP 800-82 Rev 2 Low Baseline
  - NIST SP 800-82 Rev 2 Moderate Baseline
  - NIST SP 800-82 Rev 2 High Baseline
  - NIST SP 800-171 Rev 2
  - NIST SP 800-190 2017
  - NIST SP 800-218 v1.1

- Federal Risk and Authorization Management Program (FedRAMP)
  - FedRAMP Rev 4 LI-SaaS Baseline
  - FedRAMP Rev 4 Low Baseline
  - FedRAMP Rev 4 Moderate Baseline
  - FedRAMP Rev 4 High Baseline

- Defense Information Systems Administration (DISA) Security Technical Implementation Guide (STIG)
  - DISA Docker Enterprise 2.x Linux/UNIX Security Technical Implementation Guide (STIG)
  - DISA Docker Enterprise 2.x Linux/UNIX Security Technical Implementation Guide (STIG) v2 Category I (High)
  - DISA Docker Enterprise 2.x Linux/UNIX Security Technical Implementation Guide (STIG) v2 Category II (Medium)
  - DISA Docker Enterprise 2.x Linux/UNIX Security Technical Implementation Guide (STIG) v2 Category III (Low)
  - DISA Kubernetes Security Technical Implementation Guide (STIG) Ver 1 Rel 6
  - DISA Kubernetes Security Technical Implementation Guide (STIG) Ver 1 Rel 6 Category I (High)
  - DISA Kubernetes Security Technical Implementation Guide (STIG) Ver 1 Rel 6 Category II (Medium)

- Center for Internet Security (CIS) Benchmarks
  - CIS Amazon Elastic Kubernetes Service (EKS) Benchmark v1.4.0
  - CIS Amazon Web Services Foundations Benchmark v3.0.0
  - CIS Azure Kubernetes Service (AKS) Benchmark v1.3.0
  - CIS Bottlerocket Benchmark v1.0.0
  - CIS Critical Security Controls V8
  - CIS Distribution Independent Linux Benchmark (Level 1 - Server) v2.0.0
  - CIS Distribution Independent Linux Benchmark (Level 2 - Server) v2.0.0
  - CIS Distribution Independent Linux Benchmark (Level 1 - Workstation) v2.0.0
  - CIS Distribution Independent Linux Benchmark (Level 1 - Workstation) v2.0.0
  - CIS Docker Benchmark v1.5.0
  - CIS Google Cloud Platform Foundations Benchmark v2.0.0
  - CIS Google Kubernetes Engine (GKE) Benchmark v1.4.0
  - CIS Kubernetes V1.15 Benchmark v1.5.1
  - CIS Kubernetes V1.18 Benchmark v1.6.0
  - CIS Kubernetes V1.20 Benchmark v1.0.0
  - CIS Kubernetes V1.23 Benchmark v1.0.0
  - CIS Kubernetes V1.24 Benchmark v1.0.0
  - CIS Kubernetes V1.25 Benchmark v1.7.1
  - CIS Kubernetes V1.26 Benchmark v1.8.0
  - CIS Kubernetes V1.27 Benchmark v1.9.0
  - CIS Microsoft Azure Foundations Benchmark v2.0.0
  - CIS Red Hat Enterprise Linux 8 Benchmark v3.0.0
  - CIS Red Hat Enterprise Linux 9 Benchmark v2.0.0
  - CIS Red Hat OpenShift Container Platform Benchmark v1.5.0
  - CIS Rocky Linux 9 Benchmark v2.0.0
  - CIS Ubuntu Linux 20.04 LTS Benchmark v2.0.1
  - CIS Ubuntu Linux 22.04 LTS Benchmark v2.0.0

- Amazon Web Services (AWS) Best Practices
  - AWS Foundational Security Best Practices
  - AWS Well Architected Framework

- Regulatory Compliance Standards
  - Australian Government Information Security Manual (ISM) 2022
  - BSI-Standard 200-1: Information Security Management v1.0
  - CCPA (California Consumer Privacy Act)
  - Cloud Security Alliance (CSA) Cloud Controls Matrix (CCM) Ver 4
  - Digital Operational Resilience Act (DORA) - Regulation (EU) 2022/2554
  - DPDP (Digital Personal Data Protection) Act
  - General Data Protection Regulation (GDPR)
  - HIPAA (Health Insurance Portability and Accountability Act) Security Rule 2013
  - HITRUST CSF (Health Information Trust Common Security Framework) v9.6.0
  - ISO/IEC 27001:2013 v2
  - ISO/IEC 27001:2022 v3
  - NIS2 Directive (Directive on measures for a high common level of cybersecurity across de Union) 2022/2555
  - NSA/CISA Kubernetes Hardening Guide 2022
  - PCI DSS (Payment Card Industry Data Security Standard) v3.2.1
  - PCI DSS (Payment Card Industry Data Security Standard) v4.0
  - Reserve Bank of India (RBI) Framework
  - SEBI (Securities and Exchange Board of India) Act
  - SOSC 2 (System and Organization Controls) 2010

- Risk Frameworks
  - All Posture Findings
  - MITRE ATT&CK for Enterprise v13.1
  - MITRE D3FEND v0.11.0-BETA-1

- Sysdig Best Practices
  - Sysdig Google Cloud Benchmark v1.0.0
  - Sysdig IBM Cloud Kubernetes Service (IKS) Benchmark
  - Sysdig Kubernetes
  - Sysdig Mirantis Kubernetes Engine (MKE) Benchmark
  - Sysdig Rancher Kubernetes Engine (RKE2) Benchmark

- Other Policies
  - Lockheed Martin Cyber Kill Chain
  - OWASP Kubernetes Top Ten v1.0.0

## Cloud Coverage

The following cloud services are covered:

- Amazon Web Services (AWS)
  - Amazon CloudFront
  - Amazon CloudWatch
  - Amazon DynamoDB
  - Amazon EC2
  - Amazon EC2 Auto Scaling
  - Amazon Elastic Block Store (EBS)
  - Amazon Elastic Container Registry (ECR)
  - Amazon Elastic Container Service (ECS)
  - Amazon Elastic File System (EFS)
  - Amazon Elastic Kubernetes Service (EKS)
  - Amazon ElastiCache
  - Amazon Elasticsearch Service
  - Amazon OpenSearch Service
  - Amazon RDS
  - Amazon Redshift
  - Amazon Simple Notification Service (SNS)
  - Amazon Simple Storage Service (S3)
  - Amazon VPC
  - AWS Account
  - AWS CloudFormation
  - AWS CloudTrail
  - AWS CodeBuild
  - AWS Config
  - AWS Identity and Access Management (IAM)
  - AWS Key Management Service (KMS)
  - AWS Lambda
  - AWS Region
  - AWS Secrets Manager
  - AWS VPN
  - Elastic Load Balancing (ELB)

- Google Cloud
  - Anthos
  - API Gateway
  - App Engine
  - Artifact Registry
  - Assured Workloads
  - BeyondCorp Enterprise
  - BigQuery
  - Certificate Authority Service
  - Cloud Bigtable
  - Cloud Composer
  - Cloud Data Fusion
  - Cloud Data Loss Prevention
  - Cloud DNS
  - Cloud Domains
  - Cloud Functions
  - Cloud Healthcare API
  - Cloud Intrusion Detection System (IDS)
  - Cloud Key Management Service (KMS)
  - Cloud Logging
  - Cloud Monitoring
  - Cloud Resource Manager
  - Cloud Run
  - Cloud Spanner
  - Cloud SQL
  - Cloud Storage
  - Cloud TPUs
  - Compute Engine
  - Container Engine
  - Container Registry
  - Database Migration Service
  - Dataflow
  - Dataplex
  - Dataproc
  - Datastream
  - Deployment Manager
  - Dialogflow
  - Document AI
  - Eventarc
  - Filestore
  - Firestore
  - Game Servers
  - Google Cloud Billing API
  - Google Cloud Virtual Network
  - Google Kubernetes Engine (GKE)
  - Identity and Access Management (IAM)
  - Integration Connectors
  - Managed Service for Microsoft Active Directory (Managed Microsoft AD)
  - Memorystore
  - Network Connectivity
  - Network Management
  - Network Services
  - Organization Policy API
  - Pub/Sub
  - Secret Manager
  - Service Directory
  - Service Management API
  - Speech-to-Text
  - Transcoder API
  - Vertex AI
  - Virtual Private Cloud (VPC)
  - Workflows

- Microsoft Azure
  - AKS
  - AppService
  - Authorization
  - Compute
  - Event Hub
  - Key Vault
  - Logging
  - Managed Identity
  - Monitor
  - MySQL
  - Network
  - Operational Insights
  - Operations Management
  - PostgreSQL
  - Security
  - Service Bus
  - SQL
  - Storage
  - Subscription
  - Web

## Legacy Compliance Versions

Note that you may have a [legacy version](/en/docs/sysdig-secure/posture/compliance/legacy-versions/) of Compliance installed.

- If you are an On-Prem or IBM Cloud user, see [Legacy Compliance](/en/docs/sysdig-secure/posture/compliance/legacy-versions/).

- If you are running older versions of Sysdig Secure, you may encounter different Compliance UI and features.

- Compliance and legacy Unified Compliance can be run in parallel. When benchmarks reach End of Life (EOL), data collection will only be available through Compliance, while the Legacy Reports will remain accessible on the interface for one year from their creation date.

  Data cannot be transferred between Compliance versions.

### Migration Guide

For users migrating to the Compliance module, released January 2023:

- SaaS users that connect new data sources for Sysdig cloud accounts or Sysdig agents will automatically have the new Compliance module (previously known as “Actionable Compliance”) enabled.  

  Resources of the connected data sources will be evaluated according to CSPM/Risk and Compliance policies that are applied to zones.  Results are displayed about 5-10 minutes after connection, varying by the scale of the resources.

- If you were using [Unified Compliance](/en/docs/sysdig-secure/posture/compliance/legacy-versions/compliance-unified/):
  - On existing Kubernetes clusters,  ensure the applied helm charts are updated according to the [KSPM Components](/en/docs/installation/kspm-components/) guide.
  - For Existing GCP cloud accounts, enable the [Cloud Asset API](https://console.cloud.google.com/marketplace/product/google/cloudasset.googleapis.com).
  - The new Compliance module will be auto-enabled on your existing Cloud accounts by January 26th.
  
- The new CSPM Compliance module is not available for on-prem users; they can continue using [Unified Compliance](/en/docs/sysdig-secure/posture/compliance/legacy-versions/compliance-unified/)
