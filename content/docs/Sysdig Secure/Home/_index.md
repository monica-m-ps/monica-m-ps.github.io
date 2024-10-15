---
linkTitle: "Home"
Title: "Home"
weight: "3"
aliases:
  - /en/home.html 
  - /en/home-dash-secure
  - /en/docs/sysdig-secure/home/secure-dashboards/
  - /en/todo
  - /en/docs/sysdig-secure/home/to-do/
  - /en/home
description: "The Home page offers an at-a-glance, visual representation of the most important issues in your environment. You can view your Dashboards in the default **Home** tab and a curated list of your immediate tasks under the **Recommendations** drop-down."
---

For the Home page dashboards to display data, ensure that you complete basic onboarding and connect at least one data source; otherwise, the page provides prompts to complete the setup tasks.

## Check the Data Source Status

You can view and check a status summary of data sources at the top right of the page. These include:

* Detected cloud accounts  
* Sysdig agents status, based on nodes where agents have been or could be deployed.

#### View Cloud Accounts

Select the **AWS**, **GCP**,or **Azure** icon to connect a cloud account, or to see the status of connected accounts.

For connected accounts, you can see:

* The number of accounts, projects, or subscriptions connected
* The connection status of the cluster
* A link to the [Data Sources](/en/docs/sysdig-secure/integrations-for-sysdig-secure/data-sources/cloud-accounts/) page, where you can take action

#### View Sysdig Agents

Select the Sysdig **Agents** icon to see:

* The number of agents connected
* The nodes that require attention because their agents are out of date or almost out of date
* The agent status
* A link to the [Data Sources](/en/docs/sysdig-secure/integrations-for-sysdig-secure/data-sources/sysdig-agents/) page, where you can take action

## View Runtime Events

After you set up Threat Detection using either the [Sysdig Agent](/en/install-components-secure/) or [Agentless Threat Detection](/en/aws-agentless), you can view Runtime event trends broken down by Severity in this section.

## Check Vulnerabilities

You can view various charts in this section based on the scanning you set up in your environment, such as:

* [runtime scanner](/en/install-secure/#runtime-scanning)
* [host scanner](/en/install-secure/#vulnerability-host-scanning)
* [pipeline scanning](/en/install-secure/#vulnerability-pipeline-scanning)
* [registry scanning](/en/install-registry-scan/)
* agentless scanning

The charts highlight the most critical vulnerabilities in your environment.

![](/image/home_vuln2.png)

### Posture

To view posture data, you should set up at least one of the following: [CSPM](/en/cloud-accounts-secure/), [KSPM](/en/install-secure/#compliance), or [CIEM](/en/cloud-accounts-secure/).

Depending on what is installed, you can view a breakdown of your evaluated resources by category, trends of unused permissions, poor identity hygiene practices, and trends of passing compliance requirements for your starred policies.

You can change starred policies/compliance trends on the [Compliance page](/en/docs/sysdig-secure/posture/compliance/#star-favorite-compliance-views).

If you have not starred any policies as favorites, then the line graph displays the results from the three policies with the lowest passing scores.

## Review the Recommendations

Click **Home** and select the **Recommendations** drop-down to view the Recommendations page.

Follow Sysdig **Recommendations** and take the most impactful actions to reduce security risks in your environments. It helps to cut through the noise and focus on the most important alerts and findings.

Sysdig takes a snapshot of your resources every seven days. An ascending arrow and percentage beside a recommendation indicates the increase in failing resources as compared to the previous snapshot.

You can:

* Sort your recommendations:

  * By **Highest Priority**: Sorts recommendations by the actions that have the largest impact on reducing security risk in your environments. Recommendations are categorised into areas such as Compliance, Identity, or Setup.

  * By **Last Updated:** Sorts recommendations by what has been updated most recently. It may be either a new recommendation or an existing recommendation with new findings or failing resources.

    Any list of findings or failing resources within a recommendation will be automatically sorted by highest risk.

* Scroll the top three tasks in each category, or click **See All** to see all the recommended tasks in that category:

* Check details by clicking a task to open the details panel on the right.

* Take action from the detail panel, depending on the task type.

* **Dismiss** a task, for a period of one day up to three months.

  This applies only to the current user profile; it does not remove the task from other user’s lists.

Here are the Recommendation Types:

### Setup

Recommendations prompt certain setup tasks based on your onboarding status. These tasks include connecting data sources, setting up integrations, and taking educational product tours.

![](/image/home_setup2.png)

### Identity and Access

Identity recommendations focus on highlighting Identity and Access Management (IAM) risks based on both overly permissive Policies and risky attributes Sysdig identifies for Users and Roles.

Wherever possible, the steps to be taken are summarized directly in the panel.

#### Open Jira Tickets from Identity Recommendations

If the Sysdig Secure administrator has enabled an [integration with a JIRA ticketing](/en/jira-ticketing) system, then Identity recommendations include the option to assign the policy updates to another team member via a Jira ticket.

If you delete a Jira integration, it won’t affect the tickets you opened already.

Creation and deletion of a JIRA Integration will be noted in the [Sysdig platform audit.](/en/docs/administration/sysdig-platform-audit/).
