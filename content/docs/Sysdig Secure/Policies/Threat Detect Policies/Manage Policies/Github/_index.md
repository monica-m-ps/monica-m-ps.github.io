---
linkTitle: "Github"
title: "Github Policy"
weight: "38"
aliases:
  - /en/github-policy/
description: "Github policies evaluate Github logs in Sysdig Cloud. You can edit them, duplicate to create a custom version, or create a new list matching policy from scratch."
---

{{% callout type="note" %}}

Event notifications are generally limited to a frequency of once every five minutes. For details, see [Message Throttling in Sysdig Secure](/en/docs/administration/administration-settings/outbound-integrations/notifications-management/troubleshoot-notifications-channels/#message-throttling-in-sysdig-secure).

{{% /callout %}}

## Create a Github Policy

To create a Github policy:

1. Log in to Sysdig Secure and select **Policies** > **Threat Detection** > **Runtime Policies**.

2. Click **Add Policy** and select **Github**.

## Configure a Github Policy

### Basic Parameters

**Name**: Enter a policy name.

**Description**: Provide a meaningful and searchable description.

**Enabled/Disabled**: Toggle to enable the policy so that it generates events.

**Severity**: Choose the appropriate severity level as you would like to see it in the Runtime Policies UI: **High, Medium, Low, Info**

Policy severity is subjective and is used to group policies within a Sysdig Secure instance. There is no inheritance between the underlying rule priorities and the severity you assign to the policy.

**Scope**: Define the scope to which the policy will apply, based on the type-dependent options listed.

**Link to Runbook**: (Optional) Enter the URL of a company procedure that should be followed for events resulting from this policy. For example: `https://www.mycompany.com/our-runbook-link`.

If you enter a value here, then a **View Runbook** option will be displayed in any corresponding Event.

### Policy Rules

Add or edit policy rules as needed. You can choose to **Import from Library** or to create a **New Rule**. To learn more about rules, see [Manage Threat Detection Rules](/en/manage-rules).

### Actions

Determine what should be done if a Policy is violated. 

#### Notify

Select a notification channel from the drop-down list to send notifications of events to appropriate personnel.

See [Set Up Notification Channels](/en/docs/administration/administration-settings/notifications-management/set-up-notification-channels/#set-up-notification-channels) for more information.

## Search for Existing Policies

To review the existing Workload policies: 

1. Log in to Sysdig Secure and select **Policies** > **Threat Detection** > **Runtime Policies**.

2. Filter for **Managed Policy** and **Github**. 

3. You can edit a managed policy, duplicate it to create a custom policy, or click **+ Add Policy**, and choose **Github** to configure it from scratch.