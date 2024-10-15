---
linkTitle: "AWS Elastic Container Registry Scanner"
title: "AWS Elastic  Registry Scanner"
weight: "13"
no_list: true
aliases:
  - /en/aws-eleastic-container-registry
---

### AWS ECR Single Account

{{% callout type="tip" %}}

Use the single account AWS Elastic Container Registry (ECR) installation to target one registry per installation.
<br/>To onboard an entire organization containing several member accounts, check the [AWS ECR Organizational](#aws-ecr-organizational) setup

{{% /callout %}}

Use one of the following installation methods based on where the Kubernetes cluster is located:

#### Same AWS Account

If Elastic Kubernetes Service (EKS) and the registry are in the same account, launch the installation in that account with an AWS EKS. No specific credential setup is required, since EKS already grants `AmazonEC2ContainerRegistryReadOnly` policy to the default `nodes-eks-node-group-*` role.

```bash
$ helm upgrade --install registry-scanner sysdig/registry-scanner --version=1 \
--set config.secureBaseURL=<SYSDIG_SECURE_URL> \
--set config.secureAPIToken=<SYSDIG_SECURE_API_TOKEN> \
--set config.registryType=ecr \
--set config.registryURL=<AWS_REGISTRY_URL> \
--set config.aws.region=<AWS_REGION>
```

- `<AWS_REGION>`: Registry AWS region
- `<AWS_REGISTRY_URL>`: Registry URL, such as `123456789012.dkr.ecr.us-east-1.amazonaws.com`.

#### Different Account or Kubernetes Not in AWS

Credentials are required in this case:

```bash
$ helm upgrade --install registry-scanner sysdig/registry-scanner --version=1 \
--set config.secureBaseURL=<SYSDIG_SECURE_URL> \
--set config.secureAPIToken=<SYSDIG_SECURE_API_TOKEN> \
--set config.registryType=ecr \
--set config.registryURL=<AWS_REGISTRY_URL> \
--set config.aws.region=<AWS_REGION> \
--set config.aws.accessKeyId=<AWS_ACCESS_KEY_ID> \
--set config.aws.secretAccessKey=<AWS_SECRET_ACCESS_KEY>
```

- `<AWS_REGION>`: Registry AWS region

- `<AWS_REGISTRY_URL>`: Registry URL, such as `123456789012.dkr.ecr.us-east-1.amazonaws.com`.

- `<AWS_ACCESS_KEY_ID>`, `<AWS_SECRET_ACCESS_KEY>` from an AWS user with the following permissions for the given registry to be scanned:

  ```
  ecr:GetAuthorizationToken
  ecr:BatchCheckLayerAvailability
  ecr:GetDownloadUrlForLayer
  ecr:GetRepositoryPolicy
  ecr:DescribeRepositories
  ecr:ListImages
  ecr:DescribeImages
  ecr:BatchGetImage
  ecr:GetLifecyclePolicy
  ecr:GetLifecyclePolicyPreview
  ecr:ListTagsForResource
  ecr:DescribeImageScanFindings
  ```

#### Limitations

- The scanner for single-account will target a single registry; in other words, the registry of one region. To scan several regions, either use the AWS Organizational scanner or set up one scanner per region.
- If the AWS Kubernetes cluster and the registry are in different accounts but under the same organization,  use  the AWS Organizational setup and allow listing a specific account.

### AWS ECR Organizational

AWS organizational accounts have a management account with multiple member accounts and multiple registries or regions. This option allows scanning of all registries on any member account or the region of an AWS Organization.  The Kubernetes cluster that the registry will scan may be inside or outside of AWS.

#### Process Overview

You will:

1. Set up roles and permissions in the AWS console to be used for registry scanning.
   - Optionally, validate the setup with a Sysdig test script
   - Optionally, add a configuration to the Helm chart to limit the member accounts that will be checked for scanning.
2. Install with the Helm chart in one of two ways:
   - **Option 1: Access Key/Secret Key** With an AWS Identity Access Management (IAM) user security credentials, aimed for quick testing or a more simple setup.
   - **Option 2: Service Account** With AWS Kubernetes Service Account, aimed for a more productive scenario.

#### Set Up Permissions in AWS

 In the AWS console, set up authentication roles to allow discoverability.

You will:

- Create one role in the management account (`ORGANIZATION_MANAGEMENT_ROLE_ARN`).
- A recommended step is to create a role name to be assigned to every targeted member account (`ORGANIZATION_MEMBER_ACCOUNTS_ROLE_NAME`).
  Otherwise, the default AWS member role name `OrganizationAccountAccessRole` will be used, which includes `admin` privileges.
    - Tip: To narrow the list of member accounts that will be checked during scanning, you can set the `aws.AllowListMemberAccountIDs` attribute  option in the [Registry Scanner](https://charts.sysdig.com/charts/registry-scanner/) Helm chart. This eliminates noisy error messages.
- Optionally, validate the setup using [Sysdig's test script](#step-4-optional-validate-setup-with-test-script).

##### Create Roles

- **Management role:** `ORGANIZATION_MANAGEMENT_ROLE_ARN`  will be assumed and used to discover member accounts.
- **Member role name:** `ORGANIZATION_MEMBER_ACCOUNTS_ROLE_NAME` will be assumed on the member accounts to discover and scan their registries.

##### Providing Credentials to the Registry Scanner

The registry scanner requires three types of credentials to discover and read Amazon Elastic Container Registry (ECR) images from all member accounts and all regions within an AWS organization.

![registry aws organizational](/image/registry-scanner-aws-org.png)

###### Kubernetes Job/Pod AWS Credentials

You need to provide either an Access Key/Secret Key or a service account that has the necessary permissions.

- **Access Key/Secret Key**

    If you choose to provide an Access Key/Secret Key, use the `<AWS_ACCESS_KEY_ID>`, `<AWS_SECRET_ACCESS_KEY>` format. Then, you need to set the policy permissions required to allow these credentials to impersonate the `ORGANIZATION_MANAGEMENT_ROLE_ARN`.

    Use the following policy to allow the impersonation:

    ```json
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Sid": "AllowAssumeManagementRole",
          "Effect": "Allow",
          "Action": "sts:AssumeRole",
          "Resource": "<ORGANIZATION_MANAGEMENT_ROLE_ARN>"
        }
      ]
    }
    ```

- **Service Account**

    For Amazon Elastic Kubernetes Service (EKS) clusters on AWS, [you can configure the necessary permissions at the service account level](https://docs.aws.amazon.com/eks/latest/userguide/iam-roles-for-service-accounts.html).

    Important points:

    1. Create an IAM OpenID Connect (OIDC) provider for your EKS cluster.
        **Note:** If you deployed the cluster with the [EKS Terraform module](https://registry.terraform.io/modules/terraform-aws-modules/eks/aws/latest), it was created automatically (check `oidc_provider` output).

    2. In the role associated with the service account (`SERVICE_ACCOUNT_IAM_ROLE_ARN`), create a policy to enable assuming the role of the management account.

        ```json
        {
          "Statement": [
            {
              "Action": "sts:AssumeRole",
              "Effect": "Allow",
              "Resource": "<ORGANIZATION_MANAGEMENT_ROLE_ARN>",
              "Sid": ""
            }
          ],
          "Version": "2012-10-17"
        }
        ```

    3. In the role associated with the service account (`SERVICE_ACCOUNT_IAM_ROLE_ARN`), configure a trust relationship to allow the service account in Kubernetes to assume the role.
        - Default `SERVICE_ACCOUNT_NAME` will be `registry-scanner`, derived from the helm installation release name.

        ```json
        {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Principal": {
                      "Federated": "arn:aws:iam::<MANAGEMENT_ACCOUNT>:oidc-provider/<OIDC_PROVIDER>"
                    },
                    "Action": "sts:AssumeRoleWithWebIdentity",
                    "Condition": {
                      "StringEquals": {
                        "<OIDC_PROVIDER_URL>:sub": "system:serviceaccount:<NAMESPACE>:<SERVICE_ACCOUNT_NAME>",
                        "<OIDC_PROVIDER_URL>:aud": "sts.amazonaws.com"
                      }
                    }
                }
            ]
        }
        ```

    4. Save the role `<SERVICE_ACCOUNT_IAM_ROLE_ARN>` value associated with the Kubernetes service-account and use it during the installation.

###### Credentials for the Management Role

Provide credentials for the `<ORGANIZATION_MANAGEMENT_ROLE_ARN>` to discover registries for all member accounts and all regions. You must assign policy permissions and trust relationships to this role.

**Assign Policy Permissions**

Assign one of the following policy permissions to the management role:

- `AWSOrganizationsReadOnlyAccess`: AWS managed policy which `Provides read-only access to AWS Organizations`.
- Inline policy to allow the management role to assume member account roles:

    ```json
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Sid": "AllowManagementRoleToAssumeMember",
          "Effect": "Allow",
          "Action": "sts:AssumeRole",
          "Resource": "arn:aws:iam::*:role/<ORGANIZATION_MEMBER_ACCOUNTS_ROLE_NAME>"
        }
      ]
    }
    ```

**Assign the Trust Relationship**

You must assign the trust relationship to allow this role to be assumed by the principal of the `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` / service account configured in the previous step.

- **Access Key/Secret Key**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "<AWS_ACCESS_USER_ARN>"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

- **Service Account**

    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Principal": {
                  "AWS": "<SERVICE_ACCOUNT_IAM_ROLE_ARN>"
                },
                "Action": "sts:AssumeRole"
            }
        ]
    }
    
    ```

###### Credentials for the Member Role Name

You need to provide credentials for the `<ORGANIZATION_MEMBER_ACCOUNTS_ROLE_NAME>` that is available in all scoped member accounts of the organization with access to discover and read registries within.
This role will be assumed by the `ORGANIZATION_MANAGEMENT_ROLE_ARN` to impersonate each member account.

By default, the scanner uses the [AWS-created administration role](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_accounts_access.html) `OrganizationAccountAccessRole`. Using this role will work fine; however, if you need it, you can create a lower-permission role.

**(Optional) Least-Privilege-Access Permissions**

Create a role in all accounts and assign the following least-privilege-access permissions to it.

The permissions must be assigned to all roles in all accounts that will be scanned.

- `AmazonEC2ContainerRegistryReadOnly`: AWS managed policy that `Provides read-only access to Amazon EC2 Container Registry repositories`.
Assign the following least-privilege-access permissions to the member role name:
- Custom permissions to allow region discovery:

    ```json
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Sid": "CustomPolicyAllowDescribeRegions",
          "Effect": "Allow",
          "Action": "EC2:DescribeRegions",
          "Resource": "*"
        }
      ]
    }
    ```

- The member role must be assumable by the `ORGANIZATION_MANAGEMENT_ROLE_ARN`, so you must assign the following trust relationship to the new role:

    ```json
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Effect": "Allow",
          "Principal": {
            "AWS": "<ORGANIZATION_MANAGEMENT_ROLE_ARN>"
          },
          "Action": "sts:AssumeRole"
        }
      ]
    }
    ```

Also, the `ORGANIZATION_MANAGEMENT_ROLE_ARN` must be configured to allow assuming the `ORGANIZATION_MEMBER_ACCOUNTS_ROLE_NAME` you just created in all accounts. You must assign the following policy permission to the management role, which will allow it to assume the member role in all accounts:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowManagementRoleToAssumeMember",
      "Effect": "Allow",
      "Action": "sts:AssumeRole",
      "Resource": "arn:aws:iam::*:role/<ORGANIZATION_MEMBER_ACCOUNTS_ROLE_NAME>"
    }
  ]
}
```

Optionally, if you want to test the role with the [validation script](#step-4-optional-validate-setup-with-test-script) of the next section you will need to add the following permissions:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "CustomPolicyAllowDescribeRegions",
      "Effect": "Allow",
      "Action": "iam:SimulatePrincipalPolicy",
      "Resource": "*"
    }
  ]
}
```

###### (Optional) Validate Setup with Test Script

Sysdig provides the test script  **[aws-org-test-credentials.sh](https://cf-templates-cloudvision-ci.s3.eu-west-1.amazonaws.com/registry-scanner-utils/aws-org-test-credentials.sh)** to check the created roles and permissions, along with validating the discoverables registries throughout the organization.

Launch the script from a shell. Use the AWS authentication credentials for the member account where the Registry Scanner will be deployed.

Note: If you're running it with some other credentials from the member account, remember that a trust relationship must exist with the Management Account Role. See **Assign the Trust Relationship** under [Credentials for the Management Role](#credentials-for-the-management-role).

```
./aws-org-test-credentials.sh <ORGANIZATION_MANAGEMENT_ROLE_ARN> <ORGANIZATION_MEMBER_ACCOUNTS_ROLE_NAME>
```

Sample output:

```
> using *** as member accountID
> Organization accountID: *** with discovered member account IDs
***
-------------------------
> Starting registry discovery and permission checkup

> Account ***
Checking AWS registry permissions
Discovering registries
...
-------------------------
> Discovered Registries
***.dkr.ecr.eu-central-1.amazonaws.com
***.dkr.ecr.eu-west-1.amazonaws.com
...
```

#### Install with Helm Chart

Use one of the following installation options depending on the credential format used:

##### Access Key/Secret Key

Suitable for testing or if your Kubernetes cluster is outside AWS.

`<AWS_ACCESS_KEY_ID>`, `<AWS_SECRET_ACCESS_KEY>` from the AWS user

```bash
$ helm upgrade --install registry-scanner sysdig/registry-scanner --version=1 \
--set config.secureBaseURL=<SYSDIG_SECURE_URL> \
--set config.secureAPIToken=<SYSDIG_SECURE_API_TOKEN> \
--set config.registryType=ecr \
--set config.aws.accessKeyId=<AWS_ACCESS_KEY_ID> \
--set config.aws.secretAccessKey=<AWS_SECRET_ACCESS_KEY> \
--set config.aws.managementAccountRoleARN=<ORGANIZATION_MANAGEMENT_ROLE_ARN> \
--set config.aws.memberAccountsRoleName=<ORGANIZATION_MEMBER_ACCOUNTS_ROLE_NAME>
```

##### Service Account

If you're using AWS Elastic Kubernetes Service (EKS), you must assign an IAM role (`SERVICE_ACCOUNT_IAM_ROLE_ARN`) to a **Service Account**, with which the scanner will run.

```bash
$ helm upgrade --install registry-scanner sysdig/registry-scanner --version=1 \
--set config.secureBaseURL=<SYSDIG_SECURE_URL> \
--set config.secureAPIToken=<SYSDIG_SECURE_API_TOKEN> \
--set config.registryType=ecr \
--set serviceAccount.annotations.eks\\.amazonaws\\.com/role-arn=<SERVICE_ACCOUNT_IAM_ROLE_ARN> \
--set config.aws.managementAccountRoleARN=<ORGANIZATION_MANAGEMENT_ROLE_ARN> \
--set config.aws.memberAccountsRoleName=<ORGANIZATION_MEMBER_ACCOUNTS_ROLE_NAME>
```

#### Additional Helm Configuration

##### Limit Registry Accounts to Scan

Optionally, limit the scoped registry accounts using the `allowListMemberAccountIDs` Helm chart attribute. This approach is preferred for initial testing, when the  `ORGANIZATION_MEMBER_ACCOUNTS_ROLE_NAME` may be provisioned only on select accounts and having the scanner check the rest of the member accounts in the organization would generate noisy error messages.

Set the attributes:

```bash
--set config.aws.allowListMemberAccountIDs[0]='<ORG_MEMBER_ACCOUNT_ID_1>' \
--set config.aws.allowListMemberAccountIDs[1]='<ORG_MEMBER_ACCOUNT_ID_2>' \
```

#### Limitations

- The scanner for AWS Organizational accounts does not support ECRs hosted in the management account, as per [AWS Guidelines best practices for management accounts](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_best-practices_mgmt-acct.html#best-practices_mgmt-use)
- Public registries are not supported.
