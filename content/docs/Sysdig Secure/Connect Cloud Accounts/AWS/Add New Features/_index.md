---
linkTitle: "Add New Features"
title: "Add New Features"
weight: "10"
no_list: true
aliases:
  - /en/aws/add-new-features
description: "After you connect your AWS environment to Sysdig, you can add additional features such as Cloud Detection and Response (CDR) or Vulnerability Management Host Scanning."
---

{{< callout type="info" >}}

Sysdig released a new onboarding experience for AWS in September 2024. If you onboarded your AWS Organization or Account before September 30, 2024, and would like to add more features, contact your Sysdig representative.
{{% /callout %}}

## Configure Cloud Detection and Response (CDR)

The Agentless Cloud Detection and Response (CDR) feature provides threat detection for your assets on AWS by leveraging CloudTrail logs, GuardDuty findings and S3 notifications. The feature relies on [AWS EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/index.html) to access [AWS service events](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-service-event.html#eb-service-event-cloudtrail), and analyzes threats through Falco-based policies, rules, and machine learning policies. For additional information on resources created, see [Resources created](/en/docs/installation/sysdig-secure/connect-cloud-accounts/aws/permissions-and-resources/#resources-created-1).

### Prerequisites

- You must have an AWS Account or Organization already connected to Sysdig. See [Connecting AWS Acccounts](/en/aws-secure).
- Access to a User with the [permissions required to install](/en/docs/installation/sysdig-secure/connect-cloud-accounts/aws/permissions-and-resources/#permissions-required-to-install-1).
- You must have (at least) one CloudTrail Trail in the accounts to be monitored. GuardDuty findings are ingested if the service is enabled in the AWS Accounts.

### Set Up Log Ingestion
1. Log in to Sysdig Secure, and select **Integrations** > **Cloud Accounts** | **AWS**.
2. Select an account that is part of the Organization you would like to add features to, or the individual Account you onboarded.
  
     The detail panel appears on the right. Here, you can see a list of features.
     
3. Click **Setup** beside a desired feature to open the wizard.
4. Ensure you have the necessary permissions configured as described in the prerequisites above.
5. Verify the details of your Organization or Account where the features will be added.
6. Select the regions from which events will be sent to Sysdig. AWS logs global service events to `us-east-1` region. Without this region in your AWS setup, you might miss key events, including IAM events. Ensure you select `us-east-1` plus any other AWS regions you currently use for the account.
7. Generate and apply the Terraform code:
  1. Create a `log_ingestion.tf` file in the folder that contains your `main.tf`.
  2. Copy the snippet provided into the `log_ingestion.tf` file.
  3. Run the command:
    `terraform init && terraform apply`.

#### (Optional) CDR Monitoring of S3 buckets via Notifications

Optionally, you can enable Data Events in AWS CloudTrail through granular configuration of S3 buckets. Agentless AWS Cloud Threat Detection (CDR) can monitor operations performed on objects stored in AWS Simple Storage Service (S3) buckets through S3 notifications. To learn about supported event types, see AWS's documentation on [EventBridge](https://docs.aws.amazon.com/AmazonS3/latest/userguide/EventBridge.html). To enable this function, see [Enabling Amazon EventBridge](https://docs.aws.amazon.com/AmazonS3/latest/userguide/enable-event-notifications-eventbridge.html). Once enabled, the events from those buckets will be forwarded to Sysdig and processed using the configured policies and rules. 

#### Tune the Event Pattern of Event Bridge Rule

Cloud Detection and Response (CDR) module creates an [EventBridge rule](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-rules.html) that sends events matching the conditions defined in an [event pattern](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-patterns.html).

Sysdig provides an event pattern that matches the events covered by its out-of-the-box rules. Depending on the circumstances, you can write your own rules for events not included by default, or exclude unnecessarily noisy events.

To customize this, specify the `event_pattern` variable for the `-threat-detection` module, `single-account-threat-detection` or `organization-threat-detection`, depending on the setup. This example includes only Management CloudTrail events:

```
module "single-account-threat-detection" {
  ...
  event_pattern = <<EOF
{
  "detail-type": [
    "AWS API Call via CloudTrail"
  ],
  "detail": {
    "eventCategory": [
      "Management"
    ]
  }
}
EOF
}
```

{{< callout type="info" >}}

Tuning the pattern will change the logs that will be sent to Sysdig. This will affect Sysdig runtime visibility and threat detection capabilities.

{{% /callout %}}

### Check Connection Status

To check the connection status:

1. Log in to **Sysdig Secure** and select **Integrations** > **Cloud Accounts** | **AWS**.
2. Select your account. 

   The detail panel appears on the right.

   If the connection is successful, you will see the feature as **Connected**. This may take up to 5 minutes after deploying the Terraform.

## Configure Vulnerability Management

Vulnerability Management lets you identify and mitigate security risks to protect your software and data.

For additional information on resources created, see [Resources created](/en/docs/installation/sysdig-secure/connect-cloud-accounts/aws/permissions-and-resources/#resources-created-2).

### Prerequisites

- An AWS Account or Organization already connected to Sysdig. See [Connect an AWS Acccount](/en/aws-secure).
- Access to a User with the [permissions required to install](/en/docs/installation/sysdig-secure/connect-cloud-accounts/aws/permissions-and-resources/#permissions-required-to-install-2).

### Set Up Volume Access
1. Log in to Sysdig Secure, select **Integrations** > **Cloud Accounts** | **AWS**.
2. Select an Account that is part of the Organization you would like to add features to, or the individual Account you onboarded.
   
   The detail panel appears on the right. It displays a list of features.
3. Click **Setup** beside a desired feature to open the wizard.
4. Ensure you have the necessary permissions configured as described in the initial setup.
5. Exclude or Include Resources from Vulnerability Scanning:
 
   - You can exclude VPCs or Hosts from scans using tags.
   - For more information, see [how to include/exclude resources](#excludeinclude-resources-from-vulnerability-scanning).
6. Verify the details of your Organization or Account where the features will be added.
7. Generate and apply the Terraform code:
  1. Create a `volume_access.tf` file in the folder that contains your `main.tf`.
  2. Copy the snippet provided into the `volume_access.tf` file.
  3. Run the command:
     `terraform init && terraform apply`.
  
### Exclude/Include Resources from Vulnerability Scanning

When you connect your AWS account with Vulnerability Host Scanning, by default all Virtual Private Cloud (VPC) and Elastic Compute Cloud (EC2) Instances with root volumes in the account are included in the scan.

You can use tags to exclude specific VPCs or EC2 Instances from being scanned.

**How to exclude VPCs or Hosts:** To exclude certain VPCs or EC2 instances from being scanned, assign specific tags to them in the AWS Console or using AWS APIs.

We recommend you set these tags before initiating the scanning process. You can add tags after onboarding, but the exclusion will only take effect in subsequent scans.

**How to include Data Volumes in Scans:** By default, only root volumes of EC2 Instances are scanned.

To also include data volumes in scans, use specific tags as follows:

#### Tagging Semantics

You can use the following tags at volume, EC2, or VPC level.
You can add tagging at any time, for example, if you want to exclude/include something that was or was not scanned.

**Keys:** `sysdig:secure:scan`, `sysdig:secure:data-volumes:scan`.

**Values:** `true`, `false`

**Usage Examples**

- `“sysdig:secure:scan” : “false”` on a VPC excludes all resources in the VPC from scanning.
- `“sysdig:secure:scan” : “false”` on an EC2 Instance excludes the instance and all its volumes from scanning
- `“sysdig:secure:scan” : “true”` on a data-volume of an EC2 Instance includes such volume for scanning
- `“sysdig:secure:data-volumes:scan” : “true”` on a VPC has the same effect as applying the `“sysdig:secure:scan” : “true”` tag to all the data-volumes of all the EC2 instances in it
- `“sysdig:secure:data-volumes:scan” : “true”` on an EC2 Instance has the same effect as applying the `“sysdig:secure:scan” : “true”` tag to all its data-volumes
- `“sysdig:secure:data-volumes:scan” : “true”` on a VPC, while `“sysdig:secure:data-volumes:scan” : “false”` on an EC2 Instance of the same VPC, has the same effect as applying the `“sysdig:secure:scan” : “true”` tag to all data-volumes of all the EC2 instances within the VPC except the one explicitly excluded via the tag.

The following tags are redundant; using them will have the same effect as not having them.
This is either because Sysdig scans them by default or because the values have been overridden by a tag at a higher level, such as a VPC or an EC2 Instance.

- `“sysdig:secure:scan” : “true”` on a VPC
- `“sysdig:secure:scan” : “true”` on an EC2 Instance
- `“sysdig:secure:scan” : “true”` on the root volume of an EC2 Instance
- `“sysdig:secure:scan” : “false”` on any data-volumes of an EC2 Instance
- `“sysdig:secure:scan” : “false”` on the root volume of an EC2 Instance has no effect. The root volume is always scanned as part of the EC2 instance scan.
- `“sysdig:secure:data-volumes:scan” : “false”` on a VPC
- `“sysdig:secure:data-volumes:scan” : “false”` on an EC2 Instance
- `“sysdig:secure:data-volumes:scan” : “false”` on any data-volumes of an EC2 Instance

### Check Connection Status

To check the connection status:

1. Log in to **Sysdig Secure** and select **Integrations** > **Cloud Accounts** | **AWS**.
2. Select your account. 

   The Detail panel will open on the right.
   
   If the connection is successful, you will see the feature as **Connected**. This may take up to 5 minutes after deploying the Terraform.
