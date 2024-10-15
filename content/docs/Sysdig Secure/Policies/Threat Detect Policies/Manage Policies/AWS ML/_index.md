---
linkTitle: "AWS ML"
title: "AWS ML Policy"
weight: "40"
aliases:
  - /en/aws-ml
description: "The AWS Machine Learning (ML) policy detects anomalous AWS Console login events in connected AWS cloud accounts."
---

## Key Features

This policy:

* Extends machine learning to AWS cloud accounts to monitor whether AWS console logins follow irregular patterns and notify users about suspicious activity

* Quickly detects AWS log-ons from odd locations or different areas of the globe, as well as from unexpected browsers or OSes.
* Enables advanced machine learning detection capabilities based on CloudTrail logs
* Allows users to understand _why_ an event is considered anomalous compared to the expected behavior. Specifically, the policy provides the following info:

  - Description: What the Anomaly is about 


  - Influential Factors: Variables contributing most to anomaly 


  - Confidence Level: Probability measure of detection accuracy

The AWS ML policy is available by default in the main policies menu for all Sysdig Secure SaaS users.

{{% callout type="note" %}}

Event notifications are generally limited to a frequency of once every five minutes. For details, see [Message Throttling in Sysdig Secure](/en/docs/administration/administration-settings/outbound-integrations/notifications-management/troubleshoot-notifications-channels/#message-throttling-in-sysdig-secure).

{{% /callout %}}

## Configure AWS ML Custom Policy

In the Sysdig Secure UI:

1. Select **Policies** > **Threat Detection** > **Runtime Policies** to display the **Runtime Policies** page.

2. Click **+Add Policy** (at the top right of the page).

3. Select **AWS ML** policy type.

4. Configure the policy:

   #### Basic Parameters

   **Name**: Enter a policy name.

   **Description**: Provide a meaningful and searchable description or keep the default one.

   **Enabled/Disabled**: Toggle to enable the policy so that it generates events.

   **Severity**: Choose the appropriate severity level as you would like to see it in the Runtime Policies UI: 
 *High, Medium, Low, Info*.
   
   Policy severity is subjective and is used to group policies within a Sysdig Secure instance.

   Note: There is no inheritance between the underlying rule priorities and the severity you assign to the policy.

   **Scope**: Define the scope to which the policy will apply, based on the type-dependent options listed.

   **Link to Runbook**: (Optional) Enter the URL of a company procedure that should be followed for events resulting from this policy. For example: [https://www.mycompany.com/our-runbook-link](https://www.mycompany.com/our-runbook-link).

   If you enter a value here, then a **View Runbook** option will be displayed in any corresponding Event.

   #### Detect

   **Anomalous Console Login:** Toggle on or off and select the confidence level at which the policy should be triggered: *Default*, *Higher*, or *Highest*. 
   
   * **Default:** This is the value at which the model is tested by Sysdig's Threat Research Team.
* **Higher** and **Highest:** The higher the value chosen, the lower the chance of false positives, but the higher the chance of false negatives (i.e. missed anomalous behaviors).
#### Actions

**Notify**: Select a notification channel from the drop-down list for sending notifications of events to appropriate personnel.

See also: [Set Up Notification Channels](/en/set-up-notifications).

