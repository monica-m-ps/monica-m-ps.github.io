---
linkTitle: "Workload"
title: "Workload Policy"
weight: "10"
aliases:
  - /en/workload-policy
description: "Sysdig Secure delivers a variety of workload policies out of the box. Workload policies evaluate each system call and can be configured to take immediate action. You can edit them, duplicate to create a custom version, or create a new workload policy from scratch."
---

{{% callout type="note" %}}

Event notifications are generally limited to a frequency of once every five minutes. For details, see [Message Throttling in Sysdig Secure](/en/docs/administration/administration-settings/outbound-integrations/notifications-management/troubleshoot-notifications-channels/#message-throttling-in-sysdig-secure).

{{% /callout %}}

## Create a Workload Policy

To create a Workload policy:

1. Log in to Sysdig Secure and select **Policies** > **Threat Detection** > **Runtime Policies**.

2. Click **Add Policy** and select **Workload**.

## Configure a Workload Policy

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

#### Kill Process

Toggle **Kill Process** on to have the policy automatically kill the process that triggered the event. This works for container events and hosts, and honors the agent flag to [ignore actions at the agent](/en/configuration-library-secure/#ignore-container-actions-at-the-agent-level).

#### Containers

Select what should happen to affected containers if the policy rules are breached:

* **Nothing (alert only):** Do not change the container behavior; send a
  notification according to Notification Channel settings.
* **Kill:** Kills one or more running containers immediately.
* **Stop:** Allows a graceful shutdown (10-seconds) before killing the container.
* **Pause:** Suspends all processes in the specified containers.

For more information about stop vs kill command, see [Docker's
documentation](https://docs.docker.com/engine/reference/commandline/kill/).

{{% callout type="note" %}}

The agent can be configured to prevent **Kill**, **Pause** and **Stop** actions, regardless of the policy.

See [Ignore Container Actions at the Agent Level](/en/configuration-library-secure/#ignore-container-actions-at-the-agent-level).

{{% /callout %}}

#### Capture

Toggle **Capture** on if you want to create a capture in case of an event, and define the number of seconds before and after the event that should be in the snapshot.

As of June, 2021, you can add the Capture option to policies affecting events from both the Sysdig agent and [Fargate Serverless Agents](/en/install-ecs-fargate-secure/) Fargate serverless agents.

Note that for serverless agents, manual captures are not supported; you must toggle on the Capture option in the policy definition.

See also: [Captures](/en/docs/sysdig-secure/investigate/captures/).

#### Notify

Select a notification channel from the drop-down list to send notifications of events to appropriate personnel.

See also: [Set Up Notification Channels](/en/docs/administration/administration-settings/notifications-management/set-up-notification-channels/#set-up-notification-channels).

## Search for Existing Policies

To review the existing Workload policies:

1. Log in to Sysdig Secure and select **Policies** > **Threat Detection** > **Runtime Policies**.

2. Filter for **Managed Policy** and **Workload**.

3. You can edit a managed policy, duplicate it to create a custom policy, or click **+ Add Policy**, choose the **Workload** to configure it from scratch.
