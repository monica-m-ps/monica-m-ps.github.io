---
title: "Manage Posture Policies"
linkTitle: "Manage Posture Policies"
weight: "10"
aliases:
  - /en/using-cspm-policies.html
  - /en/using-cspm-policies-and-requirements
  - /en/manage-cspm-policies-and-requirements
  - /en/docs/sysdig-secure/policies/cspm-policies/using-cspm-policies-and-requirements/
  - /en/docs/sysdig-secure/policies/cspm-policies/using-cspm-policies-and-requirements.html
  - /en/docs/sysdig-secure/policies/posture-policies/using-posture-policies-and-requirements/
  - /en/using-posture-policies.html
  - /en/using-posture-policies
  - /en/using-cspm-policies
  - /en/manage-posture
description: "Posture Policies allow you to configure what is being evaluated by the [Compliance](https://docs.sysdig.com/en/compliance/) module in the context of compliance standards such as CIS and NIST. This topic introduces you to Sysdig Posture policies, along with the requirements and controls that comprise them. It provides the conceptual background required to create, edit, and apply compliance policies in your environment. This page outlines how to manage Posture Policies. Create a new policy from scratch, or edit existing policies."
---

With Posture policies, you can:

* Clone an existing policy and edit its metadata.
* Create, edit, and delete a custom policy.
* Create, edit, and delete requirements in a custom policy.
* Link and unlink available controls to policy requirements.

In most cases, you might want to:

* Start from an existing policy
* Create or edit some requirements
* Link or unlink some controls
* Optionally create and link custom controls
* Save under a new name
* Connect the policy to a zone

The process of policy creation is separate from activation, so you can take your time to design your policy as needed. 

## Review a Policy

To review a policy:

1. Log in to Sysdig Secure.

2. Select **Policies** > **Posture** | **Policies**.

3. Select a policy from the list to open a detailed page.

4. Select **Requirements & Controls** to view requirement groups and the nested requirements to which the controls are linked. 

5. From the right detail panel, you can enable or disable individual controls within a policy. The control will be enabled or disabled for only the targeted policy.  

6. Use the search bar, and filters to find specific controls. For details, see [Search and Filter the List](/en/docs/sysdig-secure/policies/posture-policies/posture-controls/#search-and-filter-the-list).


## Create a Custom Policy

There are two ways to create a policy:

- Create a Policy from a Duplicate
- Create a Policy from Scratch

### Create a Policy from a Duplicate

1. Log in to Sysdig Secure and **Policies** > **Posture** | **Policies**.

2. To create a Policy from a duplicate, either: 

   * Click **New Policy** and select an existing policy name  from the resulting drop-down, OR
   * Select **Duplicate** from the three-dot menu next to a listed policy.

   To create a Policy from scratch:

   * Click **New Policy** and in the **Duplicate from** dropdown, select **None, start from scratch**.

3. Edit the **Name** and **Description** and click **Save**. 

4. For duplicate policies, add, delete, or edit requirement groups and requirements, link or unlink existing controls,  publish, and choose a zone.

   For custom policies, create the requirement groups and requirements you want to use and manually link controls to them.

See [Create Requirement Groups and Requirements](#create-requirement-groups-and-requirements).


### Create Requirement Groups and Requirements

In a custom policy, requirement groups and requirements can be removed or edited and new ones can be created and added. Requirements and groups are not shared between policies; to reuse a requirement from another policy, you must create a new group and requirement and then link the controls desired.

1. Select a policy.

2. On the policy page, click **New Group**.

3. Enter the requirement group name and description and click **Save**.

   ![](/image/custom_subgroup3.png)

   The group name appears in the left panel.

4. Optionally, add a subgroup.

   1. Select a requirement group that you have created and click the three-dot menu.
   2. Select **New Subgroup.**
   3. Enter the Subgroup name and description
   4. Click **Save**.

5. Add a requirement: Select a group or subgroup, click the 3-dot menu, and select **New Requirement**.

   Enter the Requirement name and description and click **Save**.

You can now link controls to your requirements.

### Link and Unlink Controls

Once you have a requirement group and requirement, the **Link Controls** button is active.

1. Select a requirement within a requirement group in your policy.

2. Click **Link Controls** in the right panel. All available controls are displayed, with the top 20 listed first.

3. Filter for the desired controls by **Type** and **Target**.

   ![](/image/link-control.png)

4. Select the desired control and click **Link**.  Repeat as needed.

5. To unlink, from the list of linked controls, hover over a control to reveal the **Unlink** option and click it.

   ![](/image/unlink.png)

   If the policy has already been published,  confirm that you want this control to no longer be evaluated by clicking **Yes, Unlink**. This action triggers a policy re-evaluation.

### Publish the Policy

When your custom policy is complete, click the **Publish** button at the top of the policy draft page and confirm. The **Date Published** is displayed from the moment of activation.

After publication, any policy edit triggers a re-evaluation and fresh results are listed in the Compliance Views after a few minutes.

### Link the Policy to a Zone

After publication, a new policy is listed with a "Missing Zone."

To apply the policy to a zone:

1. Log in to Sysdig Secure.

2. Select **Policies** > **Posture** | **Policies**.

3. Select a policy from the list.

   The Policy Detail page appears.

4. In the top right corner, select the three-dot menu icon.

5. Select **Edit**.

6. Beside **Assigned Zones**, select the zone you wish to apply from the drop-down.

7. Click **Save**.

## Filter Linked Controls

Linked controls are filtered the same way on both the Policy page and Controls page.

For more information, see [Filter the list](/en/docs/sysdig-secure/policies/posture-policies/posture-controls/#filter-the-list).

## Edit Custom Policies

For custom policies, you can edit:

* Policy name and description
* Requirement group and requirement names, descriptions
* Add/remove requirement groups and/or requirements
* Link and unlink controls
* Activated or deactivated status

All such changes trigger a policy re-evaluation if the policy is active.

## Delete Custom Policies

Deleting an active policy deletes its history of policy evaluations as well.

1. Select a custom policy.
2. Click the three-dot menu icon in the top right corner.
3. Select **Delete**.
4. At the confirmation modal, select **Yes** to delete.
  
   A re-evaluation is triggered if the policy is active.

5. Refresh Compliance Views to see the results.

## Delete Requirements

Deleting a requirement group or requirement from a policy also deletes all associations with linked controls.  

1. Select a requirement group, subgroup, or requirement in a custom policy.
2. From the three-dot menu, choose the **Delete** option and confirm **Yes, Delete** after the warning.

A policy re-evaluation is triggered if the policy is active. Refresh Compliance Views to see the results.
