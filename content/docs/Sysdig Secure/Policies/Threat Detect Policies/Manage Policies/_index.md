---
linkTitle: "Manage Threat Detection Policies"
title: "Manage Threat Detection Policies"
weight: "10"
aliases:
  -  /en/docs/sysdig-secure/policies/manage-policies/
  - /en/manage-threat-detection-policies
  - /en/manage-policies.html
  - /en/manage-policies
description: "This topic describes Sysdig's Threat Detection policy management types. It explains how to create and edit them and also provides use cases."
---



## Types of Threat Detection Policies

There are three Threat Detection policy management types:

- **Managed Policy**: These policies are created and managed by the Sysdig Threat Research Team. Sysdig updates them regularly.
- **Managed Ruleset**: Create a Managed Ruleset by duplicating a managed policy. Managed Ruleset policies still get updates, but are more flexible.
- **Custom Policy**: Create a Custom Policy from scratch or by converting existing policies. You have full control, but are responsible for keeping rules up-to-date.

## Edit a Managed Policy

Managed Policies are created by Sysdig, and are available out of the box. You cannot create new types of Managed Policies or remove existing policies, but you can edit them.

Only certain changes can be made to a managed policy: 

- Enable/disable the policy
- Set policy scope
- Set notifications
- Disable (or re-enable) individual rules

   You might wish to disable rules that generate excessive events until the cause is investigated or an appropriate exception is put in place. 

To edit a Managed Policy:

1. Log in to Sysdig Secure and select **Policies** > **Threat Detection** > **Runtime Policies**.

2. Select a policy of the type **Managed Policy** from Runtime Policies list to expand the policy details and access the icons to edit, copy, or delete the policy.

    ![](/image/policies_detail.png)

3. Click the edit (pencil) icon.

4. Configure the available parameters.

   In a Managed Policy, you can toggle the enablement, change the scope, add a Runbook link, enable or disable individual rules, and change the notification settings.

   ![](/image/workload_edit.png)

5. **Save** your changes.

   To make other changes, such as to the name, description, or severity, you must duplicate the policy so that it becomes a **Managed Ruleset**. See [Create a Managed Ruleset Policy](#create-a-managed-ruleset-policy).

## Create a Managed Ruleset Policy

Duplicate a Managed Policy to create a Managed Ruleset policy. This allows you to create multiple policies based one set of rules, but scoped to different environments, and with different actions.

1. Log in to Sysdig Secure and select **Policies** > **Threat Detection** > **Runtime Policies**.

2. From the **Runtime Policies** list, select a Managed Policy, or a Managed Ruleset, and click the duplicate/copy icon in the details panel.

   ![](/image/okta_policy_details.png)

3. Optional: Edit any of the parameters except the rules. 
   Rules can be enabled or disabled, but they cannot be removed. To remove a rule, [create a custom policy](#create-a-custom-policy).

4. Select **Save**.

   The new policy will appear in the Runtime policy list. Under **Managed Type**, it will say **Managed Ruleset**.
   
When Sysdig updates the ruleset of the Managed Policy on which a Managed Ruleset is based, the Managed Ruleset policy will also be updated.

## Create a Custom Policy

Custom policies are not subject to ruleset updates by Sysdig's Threat Research Team. Therefore, you are responsible for ensuring the latest security detections are enabled. You can modify all fields in custom policies.

You can create a Custom Policy from scratch, or by converting a Managed Ruleset.

### Create a Custom Policy from Scratch

To create a Custom Policy from scratch:

1. Log in to Sysdig Secure and select **Policies** > **Threat Detection** > **Runtime Policies**.

2. On the Runtime Policies list page, select **+Add Policy**.

3. Select the Policy type you'd like to create. 

4. Configure the policy's parameters, such as **Name**, **Description**, and **Severity**.

     Some paramaters will vary by policy types. For specific instructions, see the particular policy page. For example, [Workload Policy](/en/workload-policy).

5. Add and edit **Policy Rules**. Some policy types come with pre-configured rules. 

6. Define **Actions** to be taken if the policy rules are breached, such as **Notify** an email or Slack channel.

7. **Enable** and **Save** the policy.

### Convert a Managed Ruleset to Custom

To convert a Managed Ruleset into a Custom Policy:

1. Select a **Managed Ruleset** policy from the Runtime Policies list and click the edit icon in the details panel. 

   To create a custom policy from a Managed Policy, you must first duplicate it so that it becomes a Managed Ruleset.

2. In the **Policy Rules** section, click **convert to a custom policy**.

   You can now edit everything about this policy, including the rules. It will not be managed or updated by the Sysdig team; if new rules are offered, you are responsible for adding them to the custom policies. 

3. Click **Save**. 

   Duplicating a custom policy creates another unmanaged custom policy. 

### Configuration Examples

Different policy types may have different configuration details, as described in the three examples linked below.

- [Workload Policy Example](/en/workload-policy) (provided out of the box, but can be created or edited)

- [Container Drift Control Policy Example](/en/drift) (always custom)

- [Machine Learning Policy Example](/en/machine-learning-policy) (always custom)


## Customize Policy Updates

Whenever a rule or policy is updated, the updates are sent out to all the Sysdig agents in your environments. If you have thousands of agents deployed, this might cause excess network activity.

To reduce network traffic, use API calls to affix tags to your Sysdig agents. You can manually control when, or whether, an agent receives policy and rule updates. This enables you to reduce the ammount of communication between agents and the Sysdig backend.

### Use API Calls

#### Affix Tags

This API request pushes policy and rule updates to agents with specified tags.

**Request**

`POST /secure/policies/v4/deployments`

```
{
  "tags": 
   {dict}
} 
```

Example:

```
{
  "tags": {
    "environment": "production",
    "version": "1.0.0",
    "service": "web"
  }
}
```

**Response**

The response from policies service will contain the unique identifier `deploymentId` to check the status:

```
{
  "deploymentId": uuid,
  "deploymentExpression": string
}
```

Example:

```
{
  "deploymentId": "abc123",
  "deploymentExpression": "environment=production,service=web,version=1.0.0"
}
```

#### Check Deployment Status

Check the status of a deployment with `deployment ID`.

Rate limit: 1 request per minute.

**Request**

`GET "/secure/policies/v4/deployments/{deploymentId}"`

**Response**

```
{
  "deploymentId": uuid,
  "status": string,
  "agentsTotal": opt[int],
  "agentsPushed": opt[int],
  "tags": dict
}
```

**Example**

If the response status is `in_progress` there is an optional field of count of agents that have completed the deployment, (`agentsPushed`),  and total expected (`agentsTotal`):

```
{
  "deploymentId": "abc123",
  "status": "in_progress",
  "agentsTotal": 100,
  "agentsPushed": 95,
  "tags": {
    "environment": "production",
    "version": "1.0.0",
    "service": "web"
  }
}
```

If the response status is complete, there are no additional counts:

```
{
  "deploymentId": "abc123",
  "status": "complete",
  "tags": {
    "environment": "production",
    "version": "1.0.0",
    "service": "web"
  }
}
```

#### Disable Automatic Agent Policy Updates

The `blackout` command stops the backend from pushing policies updates automatically to the agent.

**Status Request**

`GET /api/secure/policies/v1/blackout`

**Response**

If not enabled:

```
{"message":"Not Found","referenceId":"043353e8-fda4-446e-81fc-4b9714495495"}
```

If enabled:

```
{"DateTimeStarted":"2024-06-18 20:07:06.285582+00","StartedBy":"eric.lugo@sysdig.com"}
```

**Enable Status Request**

`PUT /api/secure/policies/v1/blackout`

**Response**

Successful:

`null`

**Disable Status Request**

`DELETE /api/secure/policies/v1/blackout`

**Response**

Successful:

`null`

## Limitations

### Kubernetes workload labels
The following Kubernetes labels are no longer supported as part of a policy scope, however you can still use these labels to search events.
* kubernetes.daemonset.name
* kubernetes.deployment.name
* kubernetes.statefulset.name
* kubernetes.replicaset.name