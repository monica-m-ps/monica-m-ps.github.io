---
linkTitle: "Jira Ticketing"
title: "Jira Ticketing"
weight: "10"
aliases:
  - /en/jira-ticketing.html
  - /en/docs/administration/administration-settings/ticketing-integration/jira-ticketing/
  - /en/jira-ticketing
Description: "Jira integration lets Sysdig users open Jira tickets within the Sysdig Secure UI and assign them to team members directly.  The webhook-based option that was previously used to integrate Jira with Vulnerability Management has been replaced by an API-based method that allows for multiple Jira instances and projects."
---

This integration works for both Jira Cloud (SaaS) and Jira Data Center (On-Prem).

## Configure Jira Ticketing Integration

### Prerequisites

* Admin access to Sysdig Secure SaaS.  

* A [Jira Cloud or Data Center](https://support.atlassian.com/migration/docs/differences-administering-jira-data-center-and-cloud/) account with the [appropriate permissions](#required-permissions). 

* A Jira API Token.

  Log in to your Jira account and [generate the API token](https://id.atlassian.com/manage-profile/security/api-tokens) from Atlassian. 
  
  {{% callout type="note" %}}
  
  - The API token must be created by the same User you input when creating a new Jira Ticketing Integration.
  - The best practice is to set up the integration with a service account email rather than an individual's email.
  
  {{% /callout %}}

#### Required Permissions 

The Administrator with the Jira API token who is setting up the integration must have the following: 

- Permission to access Jira.
- Administrator Jira global permissions, or at least: 
  - Permissions to create issues in the Jira project associated with Sysdig.
  - Permissions to create attachments in the Jira project associated with issues coming from Sysdig.

The Sysdig user who will create tickets in the UI must have one of the following: 

- **Administer Jira** global permission 
- **Browse Projects** permission for the Jira project associated with Sysdig
- **Administer Projects** permission for the Jira project associated with Sysdig

### Set Up Jira Integration

1. Log in to Sysdig Secure as an administrator and select **Integrations** > **Ticketing Integrations** or **Settings** > **Ticketing Integrations**.

2. Select **New Integration**.

   The **Connect Jira Account** window appears. 

   ![](/image/connectjira2.png)

3. Specify the following:

   * **Integration Name:** You can choose any name for the integration.
   * **Atlassian Cloud URL:** Your Jira account URL, in the format `https://myaccount.atlassian.net`.
   * **Email:** The email address of the API token holder, which matches the email used in the Jira Cloud or Data Center account.
   * **API Token:** The Jira token you have generated. Follow the links in the wizard if you do not have a token.

4. Click **Next** and in **Customize Project Settings** tab specify the following:

    * **Project**: Select your project from the dropdown. 
    * **Issue Types**: Select **Epic** and at least one other type. See the note below for more information.
    * **Issue Hierarchy**: Select the default parent and child ticket in the hierarchy of issue types.
    * **Teams**: Choose whether this integration applies to particular teams, or to all teams on your account.
    * **Jira Assignee**: Optionally, select the default assignee(s).
    * **Labels**: Optionally, select labels for the tickets.

{{% callout type="note" %}} 
Jira issue types are hierarchical, and correspond to the following values:

- Epic: 1 
- Story, Task and Bug: 0
- Subtasks: -1

When selecting issue types in the wizard, ensure two sequential levels are represented. For example, you can select Epic, Story, and Bug, but not just Task and Bug.

{{% /callout %}}

5. Click **Next** and in the **Map Statuses** tab, map Sysdig's three ticket status types to the Jira statuses **Open**, **In-Progress** and **Resolved** as desired.

6. If needed, assign custom field to the appropriate issue type in the **Select Custom Fields** tab. 

7. Click Save.

  The Jira integration will be listed on the **Ticketing Integration** page with an **Active** status.

## Test the Integration 


To use the integration, open the Sysdig Secure **Home** page and check an Vulnerability Management (VM) Remediation recommendations, as described in [Open JIRA Tickets from Identity Recommendations](/en/docs/sysdig-secure/home/#open-jira-tickets-from-identity-recommendations). 


## Legacy Jira Integration

1. Log in to Sysdig Secure as an administrator and open **Settings** > **Ticketing Integrations**.

2. Select **Jira**. 

   The **Connect Jira Account** window appears.

   ![](/image/jira-setup.png)

3. Specify the following:

   * **Integration Name:** A relevant name for the integration.
   * **Atlassian Cloud URL:** Your Jira account URL. For example, `https://myacount.atlassian.net`.
   * **Email:** The email address of the API token holder, which matches the email used in the Jira Cloud account.
   * **API Token:** The Jira token you have generated.

4. Click **Save**.

   The Jira integration will be listed on the Ticketing Integrations page with **Active** status.