---
linkTitle: "AWS"
title: "AWS"
weight: "10"
no_list: true
aliases:
  - /en/docs/installation/sysdig-secure-for-cloud/deploy-sysdig-secure-for-cloud-on-aws/
  - /en/docs/sysdig-secure/sysdig-secure-for-cloud/aws
  - /en/docs/installation/sysdig-secure/connect-cloud-accounts/aws/agentless/
  - /en/docs/installation/sysdig-secure/connect-cloud-accounts/aws/agent-based-with-ciem/troubleshoot/
  - /en/docs/installation/sysdig-secure/connect-cloud-accounts/aws/agentless/troubleshoot/
  - /en/aws-agentless
  - /en/aws-secure/
  - /en/docs/installation/sysdig-secure/connect-cloud-accounts/aws/
  - /en/docs/installation/sysdig-secure/connect-cloud-accounts/aws/agentless-install/
  - /en/docs/sysdig-secure/secure-events/threat-detection-with-aws-cloudtrail/
description: "This topic describes the process of connecting your AWS environment to Sysdig. You can connect single Accounts or entire Organizations using Terraform or Cloudformation. AWS coverage includes Cloud Security Posture Management (CSPM), Cloud Infrastructure Entitlement Management (CIEM), Cloud Detection and Response (CDR) and Vulnerability Management (VM)."
---

## Review AWS Roles and Permissions

There are two [identities](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) involved in the onboarding process:
- **Installer**: Either a User or Role that will be used to perform the onboarding. Sysdig does not have access to this identity.
- **Sysdig**: A set of IAM Roles created during onboarding with specific, less permissive permissions attached. Sysdig will be given access to these Roles.

## Prerequisites

* Sysdig Secure SaaS with Admin permissions.
* Terraform v1.3.1+ installed or access to CloudFormation.
* Access to a User or Role with the permissions required to install.

### Permissions Required to Install

The **Installer** must have at least the following roles assigned:
- For **Single Account** installations:
  - [IAMFullAccess](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/IAMFullAccess.html): This policy is required to create IAM Roles and associated permissions.
- For **Organization** installations:
  - [IAMFullAccess](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/IAMFullAccess.html): This policy is required to create IAM Roles and associated permissions.
  - [AWSOrganizationsReadOnlyAccess](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AWSOrganizationsReadOnlyAccess.html): This policy is required to list Accounts and OUIDs in your Organization.
  - [AWSCloudFormationFullAccess](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AWSCloudFormationFullAccess.html): This policy is required to create a CloudFormation StackSet that creates IAM roles in each Account in your Organization.

### Permissions Granted to Sysdig

The installation creates two IAM Roles that Sysdig can access. These Roles have the following permissions attached:
- A Role named **sysdig-secure-onboarding-XXXX** used to manage the base integration with Sysdig
  - [AWSAccountManagementReadOnlyAccess](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AWSAccountManagementReadOnlyAccess.html)
  - (Organizational install) [AWSOrganizationsReadOnlyAccess](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AWSOrganizationsReadOnlyAccess.html)
- A role named **sysdig-secure-posture-XXXX** used to collect an Inventory of cloud resources, and perform CSPM
  - [SecurityAudit](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/SecurityAudit.html)
  - A Custom IAM Policy containing the following permissions:
    - `account:GetContactInformation`
    - `elasticfilesystem:DescribeAccessPoints`
    - `lambda:GetFunction`
    - `lambda:GetRuntimeManagementConfig`
    - `macie2:ListClassificationJobs`
    - `waf-regional:ListRuleGroups`
    - `waf-regional:ListRules`

## Prepare Your Environment

### 1. Configure Installation Permissions

Ensure the User or Role you log in to AWS with has the necessary permissions to install. You can:
* Use an existing User or Role who meets the permissions requirements.
* Create a new User or Role and set up permissions.
* Add permissions to an existing User or Role.

1. Log into AWS and navigate to **IAM>Users** or **IAM>Roles** as applicable
2. Open the details of the User or Role that will be used to onboard, and select **Permissions**
3. Under **Permissions policies**, verify and add necessary policies.

### 2. Authenticate and Configure Terraform

If you are installing using CloudFormation, skip to step 3.

Configure Terraform to use AWS Credentials for the User or Role from step 1. A simple way is to use the following environment variables:
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_REGION`

For alternative ways to authenticate Terraform, see the [AWS Terraform Provider documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#authentication-and-configuration).

### 3. Collect your Account Details

#### Account ID
1. Sign in to the AWS Console. For an Organizational install, ensure you sign in to the Management account of your Organization.
2. Expand the dropdown in the top right corner of your screen and copy your Account ID.

#### (Organizational install) Organization Unit IDs
By default, your entire AWS Organization will be onboarded. If you would like to restrict the onboarding to a subset of
your Organization, you can target specific OUIDs.
1. Sign in to the AWS Console.
2. Expand the dropdown in the top right corner of your screen and select **Organization**.
3. Note down the list of OUIDs that you would like to onboard. Note: accounts are added recursively below selected OUs, even if there are child OUs within them.

## Connect AWS using the Wizard

1. Log into Sysdig Secure
2. Select **Integrations > Cloud Accounts > AWS**, and select **Add AWS Account** in the top right corner.
3. Choose whether to connect your AWS [**Organization**](#organization) or [**Single Account**](#single-account).

This enables CSPM and lets you onboard Vulnerability Management, CIEM and CDR after completing.

### Terraform

#### Organization
1. Enter your:
   * Management Account ID: The Account ID of your Organization's Management Account.
   * Primary Region: The region in which to create regional resources such as StackSets
   * (optional) OUIDs: To onboard a subset of Accounts, enter the OUIDs in a comma separated list. Leave this field blank to onboard your entire Organization
2. Generate and apply the Terraform code:
   1. Create a `main.tf` file.
   2. Copy the snippet provided into the file.
   3. Run the command:
      `terraform init && terraform apply`.

After deployment, your Accounts will appear on the Cloud Accounts page.

#### Single Account
1. Enter your:
   - Account ID: The Account ID of account you wish to onboard.
2. Generate and apply the Terraform code:
   1. Create a `main.tf` file.
   2. Copy the snippet provided into the file.
   3. Run the command:
      `terraform init && terraform apply`.

After deployment, your Account will appear on the Cloud Accounts page.

### CloudFormation Templates

1. Log in to the AWS Account where you want to deploy. For Organizational installs, log into your Organization's Management Account.
2. Enter your:
   - Account ID: The Account ID of account you wish to onboard. For Organizational installs, the Account ID of your Organization's Management Account.
   - (For Organizational) OUIDs: To onboard a subset of Accounts, enter the OUIDs in a comma separated list. Leave this field blank to onboard your entire Organizationinformation such as the requested account ID, regions and units in the Wizard screen and 
3. Click **Launch Stack**.	

   * You are redirected to the AWS Console. Follow the prompts to create the CloudFormation Stack.

   * Be sure to check the Acknowledgements in the AWS Capabilities section in the AWS Console.

4. When complete, return to the Sysdig Wizard and click **Complete**. Accounts will not appear in the Cloud Accounts page until this button is clicked.

## Validate 

To validate the successful connection of your AWS environment

1. In Sysdig Secure, select **Integrations > Cloud Accounts > AWS**.

   The **Status** column shows the overall connection status (`Connected/Partial Error/Error/Unknown`)

2. Select the desired account to review the individual services in the detail drawer.

See also:  [Cloud Accounts - AWS](/en/cloud-accounts-aws), [Add New Features - AWS](/en/aws/add-new-features)
