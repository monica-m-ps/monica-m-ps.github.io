---
linkTitle: "IAM Policies"
title: "IAM Policies"
weight: "11"
aliases:
  - /en/identity-and-access.html
  - /en/ciem-policies
  - /en/ia-ciem/
description: "The **IAM Policies** page currently displays AWS policies only. Other cloud vendors will be added over time."

---

![](/image/perm_policies_list2.png)

## Filter and Sort 

### Sortable Columns

#### Unused Permission Criticality

Values: **Critical, High, Medium, Low**`

Unused Permission Criticality focuses on unused permissions, while Permission Criticality looks at all permissions. Unused Permission Criticality is designed to help you achieve Least Permissive access.

#### Risk

Values: **Critical, High, Medium, Low**

This is a calculation of risk based on all permissions. See [Understanding Risk Scoring](/en/docs/sysdig-secure/identity/#understanding-risk-scoring) for more information.

#### % of Unused Permissions 

This shows the number of unused permissions per total permissions, shown as a percentage graph. 

When remediating, immediately target the policies with the greatest exposure and refine them according to the suggestions.

Additional information in the [Detail Drawers](#detail-drawers). 

#### Policy Type

These reflect the policy types from AWS. See also the AWS [documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html) of policy types.

* **AWS Managed:** A standalone policy that is created and administered by AWS.
* **Customer:** Customer-managed standalone policies in the user's own AWS account that the user can attach to principal entities, and change and update freely.
* **Inline:** An AWS policy created for a single IAM identity (a user, group, or role). Inline policies maintain a strict one-to-one relationship between a policy and an identity. 

#### Shared 

The number of IAM entities (**users**, **roles**, and/or **groups**) assigned to a policy.  When remediating, focus on the policies affecting the greatest number of entities and make a global policy change.

#### Highest Access

See [Understand Highest Access](/en/docs/sysdig-secure/identity/#understand-highest-access).

Values:  

* **Admin:** Admin access granted
* **Write:** Write access granted
* **Read:** Read access granted
* **Empty Access:** No permissions are granted at all

#### Findings 

Policies can be listed as **Unused** on this page. It is recommended to delete Unused policies if possible.

### Available Filters

* **Search:** Free text search on terms in the resource name
* **Unused Permission Criticalitys:** By severity
* **Cloud Accounts:** Account name/number by the cloud provider  (e.g. `AWS`)
* **Access Categories:** `Admin`, `Write`, `Read`, or `Empty Access`
* **Policy Types:** `AWS-Managed`, `Customer`, `Inline`
* **Findings:** `Unused` indicates unused policies

## Analyze and Remediate

To reduce the entitlements globally for a particular policy: 

1. Click on a policy name to open the detail drawer and open tabs as needed.

   ![](/image/iam_detail.png)

2. Under the **Remediation Strategies** tab, click **Optimize this IAM Policy with a subset of only used permissions** to resolve critical permissions issues on the policy.

   ![](/image/iam_optimize.png)

   See [Understand the Suggested Policy Changes](/en/docs/sysdig-secure/identity/#understand-the-suggested-policy-changes).

5. You can follow the isntructions, or open the adjusted policy directly in the AWS console. 

6. If you have configured the Jira Ticketing integration with Sysdig Secure, you can also open a Jira ticket to optimize the policy code. 

   See  [Jira Ticketing integration](/en/jira-ticketing). 

### Manage Policies with Detail Drawers

The IAM Policies page organizes everything around the policy. Click on a policy name to open the details drawer and subtabs.

![](/image/iam_tabs.png)

* **Summary:** Displays the critical permissions issues detected on the IAM Policy, sorted by Permission Criticality and Unused Permission Criticality. 
* **Remediation Strategies**: Displays suggested Remediations, with instructions.
* **Connected IAM Resources:** Displays the Identity and Access Management (IAM) resources connected to the policy.
* **Configuration:** Displays the policy's `yaml` configuration. You can edit, download, or copy it.

### Optimize a Policy Globally

Sysdig may suggest that you **Optimize an IAM Policy** on the IAM Policy page or subtab.  If you click the button and download the proposed policy, it should be used to replace the existing policy in your AWS Console. It will affect all entities (users, roles, groups) associated with the policy. 

In the example below, the Policy risk is **Critical**. There are 151 IAM entities listed in the Shared column, which are divided between Assigned Users, Groups, and Roles. 

![](/image/uc_critical1.png)

Click the row to view the entity list on the detail drawer. A number of the Attached Users are Inactive. 

<div style="max-width: 40%">
   <img src="/image/critical_combined.png" />
 </div>

1. On the **Remediation Strategies** tab, click **Optimize this IAM Policy..** for a streamlined policy suggestion that considers the attached entities. In this case, it reduces 5311 unused permissions. 
   ![](/image/policy_rec1.png) 
2. Upload this policy to your AWS Console and associate with appropriate users, roles, and groups. 
3. Recommended: Deactivate the old policy and potentially remove the detected inactive users in AWS. 





