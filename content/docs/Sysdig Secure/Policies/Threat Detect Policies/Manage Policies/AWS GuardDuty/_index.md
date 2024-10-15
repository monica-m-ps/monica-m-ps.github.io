---
linkTitle: "AWS GuardDuty"
title: "AWS GuardDuty Policy"
weight: "90"
aliases:
  - /en/aws-gd/
description: "Sysdig ingests GuardDuty findings from connected AWS cloud accounts with GuardDuty enabled through Agentless CDR. You can configure AWS GuardDuty managed policies to configure notifications or exclude services for which you don't require GuardDuty findings. You can also create new AWS GuardDuty custom policies, where you can manually set the event severity, as well as the policy's name and description."
---

{{% callout type="note" %}}

Event notifications are generally limited to a frequency of once every five minutes. For details, see [Message Throttling in Sysdig Secure](/en/docs/administration/administration-settings/outbound-integrations/notifications-management/troubleshoot-notifications-channels/#message-throttling-in-sysdig-secure).

{{% /callout %}}

## Prerequisites

GuardDuty findings are only available when the connected AWS cloud account has GuardDuty enabled. To enable GuardDuty on your AWS account, see [Getting started with GuardDuty](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_settingup.html).

## Create an AWS GuardDuty Policy

To create an AWS GuardDuty policy:

1. Log in to Sysdig Secure and select **Policies** > **Threat Detection** > **Runtime Policies**.

2. Select **+ Add Policy** > **AWS GuardDuty**.

   **Name**: Enter a policy name.

   **Description**: Provide a meaningful and searchable description or keep the default one.

   **Enabled/Disabled**: Toggle to enable the policy so that it generates events.

   **Severity**: Choose the severity level you would like to see in the Runtime Policies UI: **High, Medium, Low, Info**.
   
   Policy severity is subjective and is used to group policies within a Sysdig Secure instance. There is no inheritance between the underlying rule priorities and the severity you assign to the policy.

   **Scope**: Define the scope to which the policy will apply, based on the type-dependent options listed.

   **Link to Runbook**: (Optional) Enter the URL of a company procedure that should be followed for events resulting from this policy. For example: [https://www.mycompany.com/our-runbook-link](https://www.mycompany.com/our-runbook-link).

   If you enter a value here, then a **View Runbook** option will be displayed in any corresponding Event.

## Policy Rules

Add or edit policy rules as needed. You can choose to **Import from Library** or create a **New Rule**. See [Manage Threat Detection Rules](/en/manage-rules).

### Actions

Determine what should be done if a Policy is violated. 

**Notify**: Select a notification channel from the drop-down for sending notifications of events to appropriate personnel.

See [Set Up Notification Channels](/en/set-up-notifications) for more information.

## Search for Existing Policies

To review the existing AWS GuardDuty policies: 

1. Log in to Sysdig Secure and select **Policies** > **Threat Detection** > **Runtime Policies**.

2. Filter for **Managed Policy** and **AWS GuardDuty**. 

3. Either edit a managed policy and duplicate it to create a custom policy, 

   or
   
   Click **+ Add Policy**, and choose **AWS GuardDuty** to configure it from scratch.
