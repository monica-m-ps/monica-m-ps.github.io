---
linkTitle: "Git Integrations"
title: "Git Integrations"
weight: "50"
aliases:
  - /en/git-iac-security.html
  - /en/docs/sysdig-secure/iac-security/
  - /en/docs/sysdig-secure/iac-security/git-iac-scanning/
  - /en/iac-landing/
  - /en/git-iac-scanning
  - /en/git-integrations
description: "Sysdig has introduced **Git Integrations** as part of its Infrastructure as Code (IaC) solution. At this time, the integrations can be used to scan incoming Pull Requests (PRs) for security violations based on predefined policies. The results of the scanning evaluation are presented in the PR itself."
---

## Introduction

Depending on the scan results, organizations can configure their repository platform to either allow or block the merging of the PR. Additionally, the information provided within the PR highlights specific areas of concern, assisting you in timely remediation.

For more information:

* See the [Iac Supportability Matrix](/en/iac-support) to review the resources and file types currently supported.

* See [IaC Policy Controls](/en/iac-policy-controls) for policy details.

### Benefits and Use Cases

Infrastructure as Code helps move security protocols and standards down into the development pipeline, highlighting and resolving potential issues as early as possible in the development process. This benefits many players within the organization:

* Security and compliance personnel see reductions in violations and security risks
* DevOps managers can streamline processes and secure the pipeline
* Developers can detect issues early and have clear guidance on how to remediate them with minimal effort.

### Process Overview

Sysdig currently supports GitHub, Bitbucket, GitLab, and Azure DevOps integrations.

**Configure the Integration**

An administrator configures an integration from the **Git Integrations** page and sets up the parameters for the supported provider(s). When the Git integration is ready, add Git Sources that define the parts of the source to protect. 

- The **repositories** (selected from the list)
- The **folders** within each repo (or all folders using/)
- The **branch pattern** (for pull request evaluations only)

**Run Scan and Check Results**

When you configure an integration and run a scan, the Pull Request Check Report contains the results in, for example, GitHub. 

## Set up a Git Integration

1. Log in to Sysdig Secure as administrator and select **Integrations  > Git Integrations**.

   * If no integration has ever been added, the page is empty.

   * If integrations already exist, the **Git Integrations List** page is displayed showing the **Integration name**, the **Status**, and the availability of **IaC Scanning** and **Threat Detection**.

     ![](/image/iac_git_integrations2.png)

2. Click **Add Git Integration**.

3. Select the relevant integration type from the drop-down and begin the configuration, depending on the provider type.

### GitHub

{{% callout type="note" %}}

The setup steps must be completed by an Admin both of the target GitHub organization and of Sysdig Secure.

Threat detection is not available on personal accounts.

{{% /callout %}}

This configuration toggles between the Sysdig Secure interface and the GitHub interface.

From the Git Integrations List page, choose **GitHub** and:

1. Select if you want to enable both IaC and Threat Detection or only IaC.

2. Enter an **Integration Name** and click **Complete** in GitHub.

   The GitHub interface opens in a new tab.

3. Sign in to GitHub, select where to install the Sysdig GitHub app, and select the target organization.

4. Select **All Repositories** or define chosen repos and click **Install**.

   ![](/image/repo-github.png)

   You will be redirected to the **Integrations** page in Sysdig Secure when installation is complete. The **Integration Status** should show **Active**. Continue with [Validation](#validate-the-git-integration) and [Add Git Sources](#add-git-sources). 

### Bitbucket

#### Prerequisites

1. Open your Bitbucket organization and create a designated account for Sysdig.
2. Configure the account's access for the relevant workspace.
3. Create a new app password for the account:
   1. Navigate to **Personal Settings > App passwords**, then click **Create app password.**
   2.  Assign the following permissions:
      * **Account:** `Read`
      * **Repositories:** `Read, Write, Admin`
      * **Pull requests:** `Read, Write`
      * **Webhooks:** `Read and write`
   3. Click **Create**.

#### Add Bitbucket Integration

1. In Sysdig, navigate to the **Git Integration** screen.

2. Click **Add Git Integration** and choose **Bitbucket**.

3. Fill in the details including the created app password from the prerequisites step. You can find the **Workspace ID** in the Bitbucket workspace settings:

    ![](/image/bitbucket-workspace-id.png)

4. Click **Add** to complete. 

   You will be redirected to the Integrations page in Sysdig Secure when installation is complete. The **Integration Status** should show **Active**. Continue with [Validation](#validate-the-git-integration) and [Add Git Sources](#add-git-sources). 

### GitLab

#### Prerequisites in GitLab UI:

- Log in to your GitLab organization and create a designated account for Sysdig Secure
- Configure the account's access for `Projects`
- Create a **unique personal access token**, setting a:
  - Unique name for the token
  - Token expiration date
  - The following scopes for the token:
    - `api`
    - `read_repository`
    - `write_repository`
-  Copy the token value

#### Add the Integration

From the Git Integrations List page, choose **GitLab** and:

1. Enter an **Integration Name** and the **Token** from the prerequisite step.

2. Click **Test Connection,** then click **Add**.
   You will be redirected to the Integrations page in Sysdig Secure when installation is complete. The **Integration Status** should show **Active**. Continue with [Validation](#validate-the-git-integration) and [Add Git Sources](#add-git-sources). 


### Azure DevOps

#### Prerequisites in Azure DevOps UI

* Log in to your Azure DevOps organization and create a designated account for Sysdig Secure.

* **Account Access:** Configure the account's access for **Repositories** and **Projects**

* **Account Subscription Permissions:** Assign **View**, **Edit**, and **Delete** subscriptions permissions to the account.

  HINT: To grant the required subscription access using the Azure CLI:

  * **ServiceHooks Namespace:** Run `az devops security permission namespace list --output table` and record the ServiceHooks namespace ID.
  * **PublisherSecurity Token:** Run `az devops security permission update --allow-bit 7 --namespace-id {{ServiceHooks namespace Id}} --subject {{accountUserEmail}} --token PublisherSecurity --output table`

- **Personal Access Token:** Retrieve a unique personal access token
  - Record the token value
  - **Token Scope**: Set to `Custom Defined`.
  - **Code Scope:** Choose `Read`, `Write`, and `Status` permissions.
  - **Extensions Scope:** Choose `Read` permission.
  - For additional help, see the [Azure DevOps documentation](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&tabs=preview-page).

#### Add the Integration

From the Git Integrations List page, choose **Azure DevOps** and:

1. Enter an **Integration Name, Organization Name,**  and  **Personal Access Token**` from the prerequisite step.

2. Click **Test Connection**, then click **Add**.
   You will be redirected to the Integrations page in Sysdig Secure when installation is complete. The **Integration Status** should show **Active**. Continue with [Validation](#validate-the-git-integration) and [Adding Git Sources](#add-git-sources). 

## Validate the Git Integration

After adding a Git Integration, you will need to[ add Git Sources for IaC](#add-git-sources).

All configured integrations are also visible in the **Integrations > Git Integrations** page. Click an entry to open the configuration page for the integration.

The status and details of the Git Integration are displayed both at the top of the configuration page and in the list.

### Integration Status

You can review the **Status** field as a column on the Integrations List page or the configuration page. It shows any issues in the connection between Sysdig Secure and the Sysdig Git provider:

   - **Active:** Everything is working as expected.
   - **Not Installed (*Only GitHub*):** The Sysdig GitHub App is not installed.
   - **Suspended (*Only GitHub*):** The Sysdig GitHub App is suspended and needs to be resumed.

The **Threat Detection** column can have the following statuses: 

- **Not available**: Threat Detection is not supported (only GitHub currently).
- **Not Enabled**: See [Enable or Disable Threat Detection](#enable-disable-threat-detection)
- **Enabled**: Threat Detection is enabled and receiving events. 
- **Error**: See [Troubleshoot Threat Detection](#troubleshoot-threat-detection)

### Last Scanned Date

As soon as the integration is fully configured and active, a scan will be run. The **Last Scanned** field is updated after every scan (every 24 hours by default).

### Additional Options

From the Integrations List page, you can use the burger (3-dot) menu for additional options on an integration.

![](/image/git_integrations.png)

 - **Manage Integration**: Open the Git integration configuration page (click on the row in the list).

 - **Start Code Scan**: Use this option to manually trigger a scan before the default 24-hour time is reached.

 - **Enable/Disable Threat Detection** (GitHub only): See the [Enable or Disable Threat Detection paragraph](#enable-disable-threat-detection)

 - **Configure in GitHub** (GitHub Only): Open the Sysdig application configuration page in GitHub.

 - **Delete**: This action deletes the *Git integration* and all the associated sources as well.

## Add Git Sources

**Git Sources** let you define the repositories, folders, and branch patterns to scan within. First, you need to connect with a Git Provider as described above. Then, you can add Git Sources from the Git Integration configuration page:

1. Log in to Sysdig Secure, and select **Integrations** > **Git Integrations**.

2. Select a Git Integration from the list.

   The Git Integration configuration page will open.

3. Click **Add Source**.

   ![](/image/git_source1.png)

4. Provide a **Name** for the source. The name will help you identify this source and can be anything such as `Prod us-east cloud resources`.

5. Choose a **Repository** and one or more **Folders** to be scanned. The system automatically checks that valid folder names have been entered.

6. Define the **Branches** where Sysdig should run a Pull Request evaluation check. You need to provide a regular expression, and Sysdig will run the check on every branch having a name that matches the regular expression. You can use the expression `.*` to check PRs on all branches or use a fixed name such as `main`.

7. Click **Add Source**. The new source will be displayed in the list.

8. Repeat to add as many sources as needed and click **Close** when done.

## Threat Detection Option

### Enable or Disable Threat Detection

Only available for GitHub. Enable Threat Detection to to forward GitHub events to Sysdig, and perform Threat Detection on events.

1. In Sysdig Secure, go to **Integrations** > **Git Integrations**.

2. Click the three dot menu icon.

   ![](/image/git_manage.png)

3. Select **Enable/Disable Threat Detection**.

   Threat Detection will stop working if you choose **Disable**, but your data and related information for IaC scanning will remain available. 

### Threat Detection on GitHub

Sysdig relies on an additional organizational webhook to perform threat detection on your GitHub organization. GitHub sends events of different event categories through organizational webhooks. Sysdig configures the webhook to receive events of the following categories:

- Branch protection rule
- Code scanning alert
- Check run
- Check suite
- Deploy key
- Deployment
- Fork
- Membership
- Organization
- Org block
- Page build
- Public
- Pull request
- Push
- Release
- Repository
- Repository advisory
- Repository imports
- Security and analysis
- Secret scanning alert
- Secret scanning alert location
- Team
- Team add
- Workflow run

This means that you will be able to perform detection on these categories of events, either through managed or custom rules. See [GitHub documentation](https://docs.github.com/en/webhooks/webhook-events-and-payloads).

### Troubleshoot Threat Detection 

If Threat Detection for GitHub is not working as expected, troubleshoot using the following steps:

- Check the Sysdig Application is installed in your GitHub Organization.
  - Log in as the Org Administrator and navigate to **GitHub Apps** under your organization's **Developer Settings**
  - Ensure the Sysdig GitHub application is installed.
- Check the webhook is present and correctly configured
  - Log in as the Org Administrator and navigate to **Webhooks** under your organization's **Developer Settings**
  - Ensure a webhook exists with a Payload URL pointing to a Sysdig domain.
  - Click `Edit` on the webhook and ensure the following:
    - The Payload URL points to the Sysdig domain for the correct Sysdig region.
    - Under `Event Types`, `Let me select individual events` is selected, and the selected event types match the list above.
    - The webhook is active.

## Pull Request Policy Evaluation

For the branches defined in Git Sources, Sysdig will run a **Pull Request Policy Evaluation** check. The check scans the Infrastructure-as-Code files in the pull request and identifies violations against the [predefined policies](/en/docs/sysdig-secure/iac-security/iac-policy-controls/).

The result of the check contains the list of violations, their severity, and the failed resources list per file.

Example output for GitHub:

![](/image/pr_result.png)

