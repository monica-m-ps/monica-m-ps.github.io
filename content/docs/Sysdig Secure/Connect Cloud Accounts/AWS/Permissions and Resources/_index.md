---
linkTitle: "Permissions and Resources"
title: "Permissions and Resources"
weight: "10"
no_list: true
aliases:
  - /en/docs/installation/sysdig-secure/connect-cloud-accounts/aws/permissions-and-resources/
description: "This document outlines the permissions required for installing and operating various Sysdig features on AWS, as well as the resources that will be created in your AWS environment."
---

## Review AWS Roles and Permissions

### Security Principals
There are two [identities](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) involved in the onboarding process:
- **Installer**: The Identity, either a User or Role that will be used to perform the onboarding. Sysdig does not have access to this identity.
- **Sysdig**: A set of IAM Roles created during onboarding with specific, less permissive permissions attached. Sysdig will be given access to these Roles.

## Base AWS Integration - Cloud Security Posture Management (CSPM)
Agentless Cloud Security Posture Management (CSPM) assesses and manages the security posture of your cloud resources without requiring agents.

### Permissions Required to Install
The **Installer** must have at least the following policies assigned in the AWS Account or Organization's Management account:

| Policy                                                                                                                                                    | Description                                                                                                              |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| [IAMFullAccess](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/IAMFullAccess.html)                                                       | Required to create IAM Roles and associated permissions.                                                                 |
| (Organization only) [AWSCloudFormationFullAccess](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AWSCloudFormationFullAccess.html)       | This policy is required to create a CloudFormation StackSet that creates IAM roles in each Account in your Organization. |
| (Organization only) [AWSOrganizationsReadOnlyAccess](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AWSOrganizationsReadOnlyAccess.html) | This policy is required to list Accounts and OUIDs in your Organization.                                                 |

### Permissions Granted to Sysdig
The **Sysdig** IAM Roles will have the following policies attached:

| Role                                                | Policy                                                                                                                                                                                                                                                                                                                       | Description                                                                                            |
|-----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| `sysdig-secure-onboarding-XXXX`                     | [AWSAccountManagementReadOnlyAccess](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AWSAccountManagementReadOnlyAccess.html)                                                                                                                                                                                | Allows Sysdig to retreive Account Alias                                                                |
| (Organization only) `sysdig-secure-onboarding-XXXX` | [AWSOrganizationsReadOnlyAccess](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AWSOrganizationsReadOnlyAccess.html)                                                                                                                                                                                        | Allows Sysdig to list accounts in your Organization.                                                   |
| `sysdig-secure-posture-XXXX`                        | [SecurityAudit](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/SecurityAudit.html)                                                                                                                                                                                                                          | Allows Sysdig to list resources within your Account.                                                   |
| `sysdig-secure-posture-XXXX`                        | A Custom IAM Policy containing the following permissions:<br/>- `account:GetContactInformation`<br/>- `elasticfilesystem:DescribeAccessPoints`<br/>- `lambda:GetFunction`<br/>- `lambda:GetRuntimeManagementConfig`<br/>- `macie2:ListClassificationJobs`<br/>- `waf-regional:ListRuleGroups`<br/>- `waf-regional:ListRules` | Allows Sysdig to list resources within your Account that are not covered by the Security Audit policy. |

### Resources Created
The following resources will be created in your AWS Environment:

| Resource                                                    | Description                                                                                                                   |
|-------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| `aws_iam_role`                                              | IAM Role with the name `sysdig-secure-onboarding-XXXX`. This role is used to manage the lifecycle of your Sysdig integration. |
| `aws_iam_role`                                              | IAM Role with the name `sysdig-secure-posture-XXXX`. This role is used for CSPM.                                              |
| (Organization only) `aws_cloudformation_stack_set`          | Used to deploy the above Roles across all Accounts in your Organization.                                                      |
| (Organization only) `aws_cloudformation_stack_set_instance` | Used to deploy the above Roles across all Accounts in your Organization.                                                      |

## Log Ingestion
The Log Ingestion component is used to enable Cloud Detection and Response (CDR) and Cloud Infrastructure Entitlement Management (CIEM)

### Permissions Required to Install
The **Installer** must have at least the following policies assigned in the AWS Account or Organization's Management account:

| Policy                                                                                                                                                    | Description                                                                                                       |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| [IAMFullAccess](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/IAMFullAccess.html)                                                       | Required to create IAM Roles and associated permissions.                                                          |
| [AWSCloudFormationFullAccess](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AWSCloudFormationFullAccess.html)                           | Required to create a CloudFormation StackSet that creates EventBridge Rules in each Account in your Organization. |
| (Organization only) [AWSOrganizationsReadOnlyAccess](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AWSOrganizationsReadOnlyAccess.html) | This policy is required to list Accounts and OUIDs in your Organization.                                          |


### Permissions Granted to Sysdig
The **Sysdig** IAM Role will have the following policies attached:

| Role                        | Policy                                                                                                                                           | Description                                                           |
|-----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------|
| `sysdig-secure-events-XXXX` | A Custom IAM Policy containing the following permissions:<br/>- `events:PutEvents`<br/>- `events:DescribeRule`<br/>- `events:ListTargetsByRule`  | Allows Sysdig to inspect EventBridge resources to perform validation. |

### Resources Created
The following resources will be created in your AWS Environment:

| Resource                                | Description                                                                                                                                                       |
|-----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `aws_iam_role`                          | IAM Role with the name `sysdig-secure-events-XXXX`. This role is used by EventBridge to send events to Sysdig, and by Sysdig to validate EventBridge resources.   |
| `aws_iam_role`                          | IAM Role with the name `AWSCloudFormationStackSetAdministrationRoleForEB`. This role is used to deploy EventBridge Rules in the selected regions in your account. |
| `aws_iam_role`                          | IAM Role with the name `AWSCloudFormationStackSetExecutionRoleForEB`. This role is used to deploy EventBridge Rules in the selected regions in your account.      |
| `aws_cloudformation_stack_set`          | Used to deploy EventBridge Rules/Role in each Account/Region in your Organization.                                                                                |
| `aws_cloudformation_stack_set_instance` | Used to deploy EventBridge Rules/Role in each Account/Region in your Organization.                                                                                |

## Volume Access
The Volume Access component is used to enable Vulnerability Management Host Scanning (VM)

### Permissions Required to Install
The **Installer** must have at least the following policies assigned in the AWS Account or Organization's Management account:

| Policy                                                                                                                                                    | Description                                                                                                      |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------|
| [IAMFullAccess](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/IAMFullAccess.html)                                                       | Required to create IAM Roles and associated permissions.                                                         |
| [AWSCloudFormationFullAccess](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AWSCloudFormationFullAccess.html)                           | Required to create a CloudFormation StackSet that creates KMS Keys/Aliases in each Account in your Organization. |
| (Organization only) [AWSOrganizationsReadOnlyAccess](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AWSOrganizationsReadOnlyAccess.html) | This policy is required to list Accounts and OUIDs in your Organization.                                         |

### Permissions Granted to Sysdig
The **Sysdig** IAM Role will have the following policies attached:

| Role                          | Policy                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Description                             |
|-------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------|
| `sysdig-secure-scanning-XXXX` | A Custom IAM Policy containing the following permissions:`kms:ListKeys`<br/>- `kms:ListAliases`<br/>- `kms:ListResourceTags`<br/>- `kms:DescribeKey`<br/>- `kms:Encrypt`<br/>- `kms:Decrypt`<br/>- `kms:ReEncrypt*`<br/>- `kms:GenerateDataKey*`<br/>- `kms:CreateGrant`<br/>- `kms:ListGrants`<br/>- `ec2:Describe*`<br/>- `ec2:CreateSnapshot`<br/>- `ec2:CopySnapshot`<br/>- `ec2:CreateTags` with the additional constraint of `ec2:CreateAction` being equal to either `CreateSnapshot` or `CopySnapshot`<br/>- `ec2:ModifySnapshotAttribute` with the additional constraint of `ec2:Add/userId` being equal to Sysdig's Worker Account ID<br/>- `ec2:DeleteSnapshot` with the additional constraint of `aws:ResourceTag/CreatedBy` being equal to `Sysdig` (which we add when creating the Snapshot) | Allows Sysdig to copy and scan Volumes. |

### Resources Created
The following resources will be created in your AWS Environment:

| Resource                                | Description                                                                                                                                                                |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `aws_iam_role`                          | IAM Role with the name `sysdig-secure-scanning-XXXX`. This role is used to copy and scan disk snapshots.                                                                   |
| `aws_iam_role`                          | IAM Role with the name `AWSCloudFormationStackSetAdministrationRoleForScanning`. This role is used to deploy KMS Keys and Aliases in the selected regions in your account. |
| `aws_iam_role`                          | IAM Role with the name `AWSCloudFormationStackSetExecutionRoleForScanning`. This role is used to deploy KMS Keys and Aliases in the selected regions in your account.      |
| `aws_iam_policy`                        | Custom IAM Policy with the permissions detailed above.                                                                                                                     |
| `aws_iam_policy_attachment`             | Custom IAM Policy with the permissions detailed above.                                                                                                                     |
| `aws_iam_policy_document`               | Custom IAM Policy with the permissions detailed above.                                                                                                                     |
| `aws_cloudformation_stack_set`          | Used to deploy EventBridge Rules/Role in each Account/Region in your Organization.                                                                                         |
| `aws_cloudformation_stack_set_instance` | Used to deploy EventBridge Rules/Role in each Account/Region in your Organization.                                                                                         |
