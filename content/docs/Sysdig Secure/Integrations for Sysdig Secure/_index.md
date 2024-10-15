---
linkTitle: "Integrations for Sysdig Secure"
title: "Integrations for Sysdig Secure"
weight: "12"
aliases:
  - /en/integrations-for-sysdig-secure.html
  - /en/integrations-secure/
description: "The Integration menu option in Sysdig Secure provides quick-link access to multiple types of integrations: connectable data sources, outbound services such as event forwarding, and third-party integrations such  Jira or Git."
---

## Cloud Accounts

Use the links in **Cloud Accounts** to review the status and details of connected cloud accounts, and add accounts from AWS, GCP, and Azure. 

See [**Cloud Accounts**](/en/cloud-accounts/).

- [**AWS**](/en/cloud-accounts-aws): Amazon Web Services (AWS)

- [**Azure**](/en/cloud-accounts-azure): Azure

- [**GCP**](/en/cloud-accounts-gcp): Google Cloud Platform (GCP)

## Data Sources

Use the links in **Integrations | Data Sources** to review the status of your Sysdig agents, and to add agents and Cloud accounts as needed.

[**Managed Kubernetes**](/en/managed-kubernetes/): Review and add managed Kubernetes clusters detected in the connected cloud accounts. 

[**Sysdig Agents**](/en/sysdig-agents/): Deploy the Sysdig agent using a range of options.

[**Sysdig Platform Audit**](/en/platform-audit): Review the activity in the Sysdig platform by user, team, or activity type.

[**Internal Agents Dashboard**](/en/internal-agents-dash): Visible in on-premises installs only, with detailed information about the agents in the environment.

**[Agent Access Key](/en/agent-access-key)** (aka "Agent Installation"): Retrieve the agent access key assigned to this account.

## Outbound

Use the links in the  `Integrations | Outbound` menu to access Sysdig features:

[**Event Forwarding**](/en/event-forwarding/): Access instructions to forward event details to a range of external tools such as Splunk,  Elasticsearch, Syslog, etc. 

**[Capture Storage](/en/docs/administration/administration-settings/storage-configure-options-for-capture-files/)**: Link to the page for configuring S3 or custom storage for storing captures and for (optional) rapid response files. 

**[Notification Channels](/en/docs/administration/administration-settings/notifications-management/set-up-notification-channels/#set-up-notification-channels)**: **Integrations > Outbound | Notification Channels** gives a quick link to configure the notification channels in Sysdig Secure. (Sysdig Monitor notification channels must be configured separately and are accessed from the Monitor UI.)

## Third-Party

**[Git Integrations](/en/docs/sysdig-secure/iac-security/git-iac-scanning/)**: Set up an integration between Sysdig and Github, Bitbucket, GitLab, or Azure DevOps to check the compliance of a pull request during development and review and remediate results in the Sysdig Secure UI. 

**[Risk Spotlight Integration](/en/docs/sysdig-secure/integrations-for-sysdig-secure/risk-spotlight-integrations/)** (Controlled Availability, contact Sysdig Support for access)

**[Ticketing Integrations: JIRA](/en/docs/administration/administration-settings/ticketing-integration/jira-ticketing/)**: Allow users in the Sysdig UI to open JIRA tickets and assign them to team members directly.

## Additional

* Forward vulnerability scan results to ServiceNow or Jenkins
* Review sample CI/CD integrations 

### Forward Vulnerability Scan Results

**[ServiceNow](https://store.servicenow.com/sn_appstore_store.do#!/store/home)** (Tech Preview): Push the results of Sysdig vulnerability scans to an existing ServiceNow installation. [Install Guide for the Sysdig Container Vulnerability Response](https://store.servicenow.com/sn_appstore_store.do#!/store/application/11f6170f87a55110a9b74339dabb3538?referer=%2Fstore%2Fsearch%3Flistingtype%3Dallintegrations%25253Bancillary_app%25253Bcertified_apps%25253Bcontent%25253Bindustry_solution%25253Boem%25253Butility%25253Btemplate%26q%3Dsysdig&sl=sh) plugin. 

**[Jenkins plugin](/en/jenkins-integrate-new-vuln)**: Push the results of Sysdig pipeline (cli-scanner) scan to an existing Jenkins installation. [Install Guide for the Sysdig Secure Jenkins Plugin](https://plugins.jenkins.io/sysdig-secure/). 

#### Additional Examples of CI/CD Integrations 

[Azure Pipelines](https://sysdig.com/blog/image-scanning-azure-pipelines/)

[AWS Codepipeline](https://sysdig.com/blog/image-scanning-aws-codepipeline-codebuild/)

[CircleCI](https://sysdig.com/blog/image-scanning-circleci/)

[Github Actions](https://sysdig.com/blog/image-scanning-github-actions/)

[Gitlab](https://sysdig.com/blog/gitlab-ci-cd-image-scanning/)

[Tekton Pipelines](https://sysdig.com/blog/securing-tekton-pipelines-openshift/)



