---
linkTitle: "AWS CloudTrail"
title: "AWS CloudTrail Policy"
weight: "32"
aliases:
  - /en/aws-cloudtrail-policy/
description: "AWS CloudTrail policies evaluate each AWS CloudTrail entry in Sysdig Cloud. You can edit them, duplicate to create a custom version, or create a new list matching policy from scratch. These policies are used by Sysdig Agentless detection engine."
---

There are several types of managed policies under the category of AWS Cloud Trail policies, such as **Sysdig AWS Threat Intelligence** and **Sysdig AWS Notable Events**. These policies detect noteworthy occurrences, such as a suspicious IP Inbound Request, or a Console Login Without Multi-Factor Authentication (MFA).

{{% callout type="note" %}}

Event notifications are generally limited to a frequency of once every five minutes. For details, see [Message Throttling in Sysdig Secure](/en/docs/administration/administration-settings/outbound-integrations/notifications-management/troubleshoot-notifications-channels/#message-throttling-in-sysdig-secure).

{{% /callout %}}

## Behavioral Analytics

**Sysdig AWS Behavioral Analytics** is a unique case under the AWS CloudTrail policy. Instead of triggering at single incidents, its rules detect both suspicious sequences of actions and unusual clusters of activities across various AWS services, such as Identity and Access Management (IAM), Elastic Compute Cloud (EC2) and Simple Storage Service (S3). This improves detection of sophisticated threats, such as privilege escalation attempts and reconnaissance activities.

You can turn rules for **Sysdig AWS Behavioral Analytics** on or off, and create edit exceptions, but you cannot duplicate it to create custom or ruleset policies, as you can with other managed policies. This is because behavioral analytics is not based on the Falco rules engine. 

### View Behavioral Analytics

To see behavioral analytics policies:

1. Log in to Sysdig Secure.

2. Select **Policies** > **Runtime Policies**.

3. Under the **AWS CloudTrail** policy, find the managed policy type **Sysdig AWS Behavioral Analytics**.

4. Select the policy to review its rules in detail.

To see behavioral analytics events:

1. Log in to Sysdig Secure.

2. Select **Threats** > **Activity** | **Events Feed**.

   Behavioral analytics events will show up in the event feed.

   There are two types of behavioral analytics; stats and sequence. Stats will have a list of API names and a corresponding count of how often that API was hit in the time interval. Sequence events will show the sequence of APIs that were hit to match the condition of the rule. 
   
   Stateful events do not appear in the insights view. 

### Add Exceptions

You can add values to exception definitions added by the Sysdig threat research team. To add exceptions:

1. Log in to Sysdig Secure.

2. Select **Policies** > **Runtime Policies**.

3. Under the **AWS CloudTrail** policy, select **Sysdig AWS Behavioral Analytics**.

   The detail panel appears.

4. Select the edit icon in the top right.

   The editor page appears.

5. Select a rule to which you want to add an exception.

   The rule detail panel appears.

6. Select the edit icon in the top right.

   The rule page appears.

7. Select **Open Exceptions Editor**.

   The Edit Exceptions page appears.

8. Select the edit icon on an exception to begin editing.

   You can add values to exceptions defined by the Sysdig threat research team, but you cannot define your own exceptions. The exception comparator supports `regex`, `in,` and `=` field matching.
  
   Behavioral analytics do not support Suggested Exceptions and Auto-tuner.

## Create an AWS CloudTrail Policy

To create an AWS CloudTrail policy:

1. Log in to Sysdig Secure and select **Policies** > **Threat Detection** > **Runtime Policies**.

2. Click **Add Policy** and select **AWS CloudTrail**.

## Configure an AWS CloudTrail Policy

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

See also: [Set Up Notification Channels](/en/docs/administration/administration-settings/notifications-management/set-up-notification-channels/#set-up-notification-channels).

## Search for Existing Policies

To review the existing Workload policies: 

1. Log in to Sysdig Secure and select **Policies**  > **Threat Detection**  > **Runtime Policies**.

2. Filter for **Managed Policy** and **AWS CloudTrail**. 

3. You can edit a managed policy, duplicate it to create a custom policy, or click **+ Add Policy**, and choose **AWS CloudTrail** to configure it from scratch.