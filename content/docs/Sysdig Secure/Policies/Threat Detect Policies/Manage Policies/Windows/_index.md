---
linkTitle: "Windows Workload"
title: "Windows Workload Policy"
weight: "100"
aliases:
  - /en/windows_policy
description: "The Windows Workload policy uses Falco rules to evaluate system calls and can be configured to take immediate action. The Windows Workload managed policy is maintained by Sysdig's Threat Research Team. You can turn the policy on or off, and manage exceptions."
---

Whereas [Workload Policies](/en/workload-policy) are designed for Linux, Windows Workload Policies are designed for workloads running on Windows.

The Windows Workload policy comes pre-installed in Sysdig Secure. As it is a managed policy, you cannot create new instances of the policy. To read about the differences between managed and custom policies, see [Types of Threat Detection Policies](/en/docs/sysdig-secure/policies/threat-detect-policies/manage-policies/#types-of-threat-detection-policies).

Falco rule exceptions are supported out of the box.

{{% callout type="note" %}}

Event notifications are generally limited to a frequency of once every five minutes. For details, see [Message Throttling in Sysdig Secure](/en/docs/administration/administration-settings/outbound-integrations/notifications-management/troubleshoot-notifications-channels/#message-throttling-in-sysdig-secure).

{{% /callout %}}

## Enable or Disable a Windows Workload Policy

To enable or disable a Windows Workload Policy:

1. Log in to Sysdig Secure.

2. Select **Policies** > **Runtime Policies** from the left navigation bar.

  The **Runtime Policies** page appears.

3. Locate a Windows Workload policy by selecting **Windows Workload** from the **Select policy type** drop-down.

4. Use the toggle on the left to enable or disable the policy.

## Configure a Windows Workload Policy

To configure a Windows Workload Managed Policy:

1. Log in to Sysdig Secure.

2. Select **Policies** > **Runtime Policies**.

  The **Runtime Policies** page appears.

3. Locate a Windows Workload policy by selecting **Windows Workload** from the **Select policy type** drop-down.

4. Select the edit (pencil) icon on a policy listing, or select the policy to open the detail pane, and then select the edit icon.

  The Policy Detail page appears. Here you can configure the following:

  - **Enabled**: Use the toggle to enable or disable the managed policy.
  - **Link to Runbook**: (Optional) Enter the URL of a company procedure that should be followed for events resulting from this policy. For example: `https://www.mycompany.com/our-runbook-link`.

    If you enter a value here, then a **View Runbook** option will be displayed in any corresponding Event.
  
  - **Policy Rules**: Use the toggles to disable or enable individual rules. You cannot delete rules, or add additional rules to managed policies. Select a rule to open the detail panel. Here, you can select the edit icon to edit the rule.

  - **Notify**: Select a notification channel to receive alerts when the policy triggers and generates an event. See [Set Up Notification Channels](/en/docs/administration/administration-settings/notifications-management/set-up-notification-channels/#set-up-notification-channels).