---
linkTitle: "Okta ML"
title: "Okta ML"
weight: "17"
aliases:
  - /en/okta-ml
description: "The Okta Machine Learning (ML) policy detects anomalous Okta login events in connected Okta cloud accounts. "
---

This policy:

* Leverages machine learning to continuously monitor your cloud environment for unusual login activities, and notifies you when they happen.

* Detects and alerts you of anomalies in your Okta logs.

* Extends the functionality of Falco rules, providing a second layer of defense.

## Prerequisites

- Okta must be integrated into Sysdig Secure. See [Okta Integration](/en/secure-okta).

- **Customer Data Analytics** collection must be enabled under **Privacy Settings**. See [Privacy Settings](/en/privacy-settings.html).

## Configure AWS ML Custom Policy

In the Sysdig Secure UI:

1. Select **Policies** > **Threat Detection** > **Runtime Policies** to display the **Runtime Policies** page.

2. Click **+Add Policy** (at the top right of the page).

3. Select **Okta ML** policy type.

4. Configure the policy:

   ![](/image/policy_awsml.png)

   #### Basic Parameters

   **Name**: Enter a policy name.

   **Description**: Provide a meaningful and searchable description or keep the default one.

   **Enabled/Disabled**: Toggle to enable the policy so that it generates events.

   **Severity**: Choose the appropriate severity level as you would like to see it in the Runtime Policies UI: 
    
    **High, Medium, Low, Info**

   Policy severity is subjective and is used to group policies within a Sysdig Secure instance.

   Note: There is no inheritance between the underlying rule priorities and the severity you assign to the policy.

   **Scope**: Define the scope to which the policy will apply, based on the type-dependent options listed.

   **Link to Runbook**: (Optional) Enter the URL of a company procedure that should be followed for events resulting from this policy. For example: `https://www.mycompany.com/our-runbook-link`.

   If you enter a value here, then a **View Runbook** option will be displayed in any corresponding Event.

   #### Detect
   
   **Anomalous Login:** Toggle on or off and select the confidence level at which the policy should be triggered: **Default**, **Higher**, or **Highest**. 

   * **Default:** This is the value at which the model is tested by Sysdig's Threat Research Team.
   * **Higher** and **Highest:** The higher the value chosen, the lower the chance of false positives, but the higher the chance of false negatives (i.e. missed anomalous behaviors).

#### Actions

**Notify**: Select a notification channel from the drop-down list for sending notifications of events to appropriate personnel.

See [Set Up Notification Channels](/en/set-up-notifications).
