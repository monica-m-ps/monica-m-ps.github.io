---
linkTitle: "Forwarding to Amazon SQS"
title: "Forwarding to Amazon SQS"
weight: "34"
aliases:
  - /en/forwarding-to-amazon-sqs.html
description: "With Amazon Simple Queue Service (SQS) event forwarding, you can send, store, and receive events from Sysdig in an SQS queue and route them to other services in Amazon Web Services (AWS)."
---
## Prerequisites

To foward events to Amazon SQS  you will need:

* An SQS queue
* An Identity and Access Management (IAM) user
* An Access Key and Secret to authenticate Sysdig as that IAM user
* Permission for the IAM user to write messages on the target queue

## Configure on AWS

Prepare your AWS Console for Event Forwarding to Amazon SQS. You will need an IAM User, an Access Key and Secret for authentication, and an SQS queue.

1. Create or identify a target AWS IAM User you want to give Sysdig access to. We recommend you create a new user for security reasons. See [Creating an IAM user in your AWS account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html).

2. Take note of the Amazon Resource Name (ARN) for the IAM User. Its format will resemble `arn:aws:iam::111111111111:user/sysdig-efo-user`. You will need to input these later to configure the permissions to access the SQS queue.

3. Create an Access Key and Secret Key for the user. See [Managing access keys for IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html).

4. Take note of the Access Key and Secret Key. You will need to input these later in the Sysdig UI.

5. Create or identify a target SQS Queue. See [Getting started with Amazon SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-getting-started.html).
  
6. Take note of the Amazon Resource Name (ARN) for the SQS Queue. Its format will resemble `arn:aws:sqs:us-west-2:222222222222:sysdig-efo-queue`. You will need to input this later in the Sysdig UI.

7. Configure the Access Policy for the queue to allow the target AWS IAM User to perform `SQS:SendMessage`, `sqs:ListQueues` and `sqs:GetQueueUrl` on that queue. See [Configuring access policy](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-configure-add-permissions.html).

    If the SQS queue and the IAM User are on different accounts, the Amazon Resource Names (ARNs) will reference different accounts. For example:
    * 111111111111 is the AWS IAM user account.
    * 222222222222 is the SQS queue `ownerAccount`.
    They will instead be the same value if the resources reside on the same AWS account.

    Your configuration would look like this:

```json
{
  "Version": "2012-10-17",
  "Id": "__default_policy_ID",
  "Statement": [
    {
      "Sid": "__sender_statement",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::111111111111:user/sysdig-efo-user"
      },
      "Action": [
        "sqs:GetQueueUrl",
        "sqs:SendMessage"
      ],
      "Resource": "arn:aws:sqs:us-east-1:222222222222:efo-queue"
    }
  ]
}
```

## Configure Event Forwarding

1. Log in to Sysdig Secure as Admin and select **Settings** > **Event Forwarding**.
2. Click **+Add Integration** and choose **Amazon SQS** from the dropdown.
3. Configure the required options:

     * **Integration Name:** Choose an integration name, for example, `sysdig-efo-queue`.
     * **Access Key:** Enter your IAM user's Access Key.
     * **Access Secret**: Enter your IAM user's Secret Key.
     * **Owner Account**: Enter the AWS account where the SQS queue is. If unspecified, Sysdig assumes it's the same account as that of the IAM user.
     * **Region:**  Enter the AWS region where you created you Amazon SQS, for example, `us-west-2`.
     * **Queue:** Enter the name of the target Amazon SQS queue. Note, this is not the full URL or the ARN, but just the name. For example: `sysdig-efo-queue`.
     * **Delay** Optional: Enter a value (in seconds) between 0 and 900 that a message delivery should be delayed.
      * **Metadata** Optional: Set up to 10 key value headers with which the messages should be tagged. Entries can be string values.
     * **Data to Send:** Select from the dropdown the types of Sysdig data that should be forwarded. The available list depends on the Sysdig features and products you have enabled.

4. Toggle the **Enabled** switch as necessary. You will need to **Test Integration** with the button below before enabling the integration.
5. Click **Save**.

## Configure Agent Local Forwarding

Review the Agent Local Fowarding [configuration steps](/en/event-forwarding/#configure-agent-local-forwarding) and use the following parameters for this integration.

| **Type** | **Attribute** | **Required?** | **Type**             | **Allowed values** | **Default** | **Description**                                              |
| -------- | ------------- | ------------- | -------------------- | ------------------ | ----------- | ------------------------------------------------------------ |
| SQS      | accessKey     | yes           | string               |                    |             | Access Key for authenticating on AWS to send data on the queue |
| SQS      | accessSecret  | yes           | string               |                    |             | Access Secret for authenticating on AWS to send data on the queue |
| SQS      | ownerAccount  | no           | string    |         |     | The AWS Account where the SQS queue is. If unspecified, Sysdig assumes it’s the same account as that of the IAM user. |
| SQS      | token         | no            | string               |                    |             | Session token  for authenticating on AWS to send data on the queue |
| SQS      | region        | yes           | string               |                    |             | Region in which the SQS queue is hosted                      |
| SQS      | queue         | yes           | string               |                    |             | SQS queue name                                               |
| SQS      | delay         | no            | int                  | 0-900              | 0           | Delay, in seconds, applied to the data                       |
| SQS      | headers       | no            | sequence of mappings |                    |             | Extra headers to add to the payload. Each header mapping requires 2 keys: “key” for the header key and “value” for its value |
