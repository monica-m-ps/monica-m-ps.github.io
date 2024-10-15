---
linkTitle: "Workload ML"
title: "Workload ML Policy"
weight: "31"
aliases:
  - /en/machine-learning-policy
  - /en/workload-ml
  - /en/docs/sysdig-secure/policies/threat-detect-policies/manage-policies/machine-learning/
description: "Sysdig's Workload ML (Machine Learning) policy is used to provide a second layer of defense to complete the deep and exact coverage that Falco provides with a broader statistics-based approach."
---

## Key Features

* Machine learning collects low-level activities from your infrastructure, aggregating them over time and applying algorithms.
* With machine learning policies, you can configure the detections you want to use and their thresholds.
* Machine learning detection algorithms estimate the probability that those activities are related to the detection subjects, i.e. miners. The Sysdig Workload ML detections don't rely on mere program names or executable checksums matching. Instead, they are based on runtime behaviors.

{{% callout type="note" %}}

Event notifications are generally limited to a frequency of once every five minutes. For details, see [Message Throttling in Sysdig Secure](/en/docs/administration/administration-settings/outbound-integrations/notifications-management/troubleshoot-notifications-channels/#message-throttling-in-sysdig-secure).

{{% /callout %}}

## Configure Workload ML Custom Policy

To configure a Workload ML Custom Policy in the Sysdig Secure UI:

1. Select **Policies** > **Threat Detection** > **Runtime Policies** to display the **Runtime Policies** page.

2. Click **+Add Policy** (at the top right of the page).

3. Select the **Workload ML** policy type.

4. Configure the policy by specifying the basic parameters:

   ### Basic Parameters

   **Name**: Enter a policy name

   **Description**: Provide a meaningful and searchable description

   **Enabled/Disabled**: Toggle to enable the policy so that it generates events.

   **Severity**: Choose the appropriate severity level as you would like to see it in the Runtime Policies UI: 
    *High, Medium, Low, Info*
   
   Policy severity is subjective and is used to group policies within a Sysdig Secure instance.

   Note: There is no inheritance between the underlying rule priorities and the severity you assign to the policy.

   **Scope**: Define the scope to which the policy will apply, based on the type-dependent options listed.

   **Link to Runbook**: (Optional) Enter the URL of a company procedure that should be followed for events resulting from this policy. For example: [https://www.mycompany.com/our-runbook-link](https://www.mycompany.com/our-runbook-link).

   If you enter a value here, then a `View Runbook` option will be displayed in any corresponding Event.

   ### Detect

   You can choose what type of Machine Learning-based detections you want enable in your policy. We support only `Crypto Mining Detection` at this time. 

   **Crypto Mining Detection:** The supported detection type 

   **Confidence Level Enablement:** Fine-tune the policy to choose at which certainty level the detection should trigger an event.

   **Severity** defined at detection level, so that you can have a different severity for each detection type.

   ### Actions

   **Notify**: Select a notification channel from the drop-down list for sending notification of events to appropriate personnel.

   See also: [Set Up Notification Channels](/en/set-up-notifications).
