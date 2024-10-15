---
linkTitle: "Troubleshoot"
title: "Troubleshoot AWS Agentless Installs"
weight: "10"
no_list: true
aliases:
  - /en/troubleshoot-aws-agentless
  - /en/docs/installation/sysdig-secure/connect-cloud-accounts/aws/agent-based-with-ciem/troubleshoot/
description: "Use these suggestions to troubleshoot an agentless AWS installation."
---

## Check for Service Control Policies
[AWS Service control policies](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html) (SCPs) can commonly hinder Sysdig's operations by denying required permissions, even if they are correctly granted at the Role level. SCPs are created at the Organization level, and may be automatically created if using services such as AWS Control Tower.

To check for SCPs that may impact Sysdig Integrations:

1) Log into the Management Account of your AWS organization, and locate the affected account in
   [the organization tree](https://console.aws.amazon.com/organizations/v2/home/accounts).
2) Click on the account, and open the `Policies` tab.
3) Under `Applied policies`, check each policy and confirm that it does not deny any of the following actions:
   * `sts:AssumeRole` from the Sysdig Trusted Identity
   
   * (If using CSPM) Any permissions defined in the [AWS SecurityAudit managed policy](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/SecurityAudit.html)
   
   * (If using Agentless Threat Detection) `events:PutEvents` on Sysdig's EventBridge Bus

## Troubleshoot CSPM
- **IAM Role:** Ensure the affected account contains an IAM Role with:
  - A TrustRelationship policy that allows `sts:AssumeRole` from Sysdig's Trusted Identity
  - The [AWS SecurityAudit managed policy](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/SecurityAudit.html) attached
- **SCPs:** If the account is part of an AWS Organization, ensure there are no SCPs that overwrite these permissions.

## Troubleshoot Agentless CDR
- **EventBridge Rule:** Ensure the affected account contains an EventBridge Rule with an associated IAM Role and EventBridge Target. These resources will all begin with a `sysdig-secure-cloudtrail` prefix.
  
  - Note that the EventBridge resources are **regional**. They must exist in each region from which you would like capture events.

- **CloudTrail Trail:** In order for CloudTrail events to be correctly routed to EventBridge, a CloudTrail trail must exist. Ensure there is an active Trail in the affected account, or an active organizational Trail if the affected account is part of an AWS Organization.
  
- **SCPs:** If the account is part of an AWS Organization, ensure there are no SCPs that restrict `sts:AssumeRole` or `events:PutEvents` for these resources.

- **Events flowing:** Check that Events are flowing through the EventBridge Rule:
  
  - Navigate to the [EventBridge Rules console](https://console.aws.amazon.com/events/home#/rules) and locate the Rule created by Sysdig.
  
  - Select this Rule, and navigate to the `Monitoring` tab. Check the `Invocations` and `FailedInvocations` graphs.
  
  - If both graphs show `no data`, this indicates that no events have occurred that will be captured by Sysdig. Make sure that there is an active CloudTrail Trail, and that there is activity occurring in the region of the EventBridge Rule.
  
  - If there is activity in the `Invocations` graph, and the `FailedInvocations` graph is non-zero, you can use the following steps to investigate the failures:
  
    **Failed Invocations:**
  
    1. In this account, create a temporary SQS Queue that will be used as a Dead Letter Queue for the EventBridge Target.
    2. Navigate to the Rule and under the `Target` tab, click Edit.
    3. Expand the `Additional settings` section and select the second option for the Dead-letter queue (*Select an Amazon SQS queue in the current AWS account to use as the dead-letter queue*).
    4. Select the SQS Queue created in step 1, and save the configuration.
    5. Now that failed events will be captured in the DLQ, ensure that some activity occurs in the account. If the account sees high usage, events may already be occurring, however if the account is relatively unused, you man need to manually trigger some events. A simple option is to create or delete an S3 bucket.
    6. Navigate to the SQS Queue console and locate your temporary Queue.
    7. In the top right corner, click `Send and recieve messages`. Under `Receive messages`, click `Poll for messages`.
    8. Select a message and view the `Attributes` tab. Details of the failed invocation will be available.
