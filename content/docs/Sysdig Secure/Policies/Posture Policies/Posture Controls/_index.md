---
linkTitle: "Posture Controls"
title: "Posture Controls"
weight: "12"
aliases:
  - /en/cspm-controls.html
  - /en/cspm-controls
  - /en/docs/sysdig-secure/policies/cspm-policies/cspm-controls/
  - /en/posture-controls.html
  - /en/posture-controls
description: "On the Posture Controls page, you can see the logic behind compliance results. Drill into the control details:"
---

* To know precisely what has been or will be evaluated
* To review a specific control to see its logic and remediation
* To edit control parameters to tune your compliance results and personalize evaluation parameters

Sysdig Posture Controls are built on the Open Policy Agent (OPA) engine, using OPA's policy language, Rego. The Posture Controls library exposes the code used to create the controls and the inputs they evaluate, providing full visibility into their logic. You can download the code as a JSON file. 

 For details, see external documentation on:
 
 -  [Open Policy Agent (OPA)](https://www.openpolicyagent.org/docs/latest/)
 - [Rego](https://www.openpolicyagent.org/docs/latest/policy-language/)

## Prerequisites

This feature requires the **Compliance** component.

If necessary, review: 

* [Evaluate and Remediate](/en/docs/sysdig-secure/posture/compliance/#evaluate-and-remediate)

#### How Controls are Structured

Sysdig controls are built on the Open Policy Agent (OPA) engine, using the OPA policy language, Rego. The Posture Controls library exposes the code used to create the controls and the inputs they evaluate, providing full visibility into their logic. You can download the code as a JSON file.

 For details, see the  [Open Policy Agent (OPA)](https://www.openpolicyagent.org/docs/latest/) and [Rego](https://www.openpolicyagent.org/docs/latest/policy-language/) external documentation.

## Navigate Posture Controls List

1. To see the Posture Controls list, select **Policies** > **Posture** | **Controls**.

2. Select a specific control to open details in the right panel.


      <div style="max-width: 70%">
         <img src="/image/posture_controls2.png" />
       </div>

### Search and Filter the List 

Find specific controls through search and several filter options.

*  **Search**: Enter any word or part of a word in the name, description, or author to find matching controls.
* Severity: Filter controls by their severity level; **H**igh, **M**edium, or **L**ow.
* **Select Type**: From the drop-down, choose a control type such as **Cluster**, **Host**, **Identity**, or **Resource**.
* **Select Target:** The specific platforms, distributions, and supported versions against which a control evaluates resources. Online cloud platforms, such as Azure Kubernetes Service (AKS), Amazon Web Services (AWS), and Google Cloud Platform (GCP), do not have versioning but always relate to the latest version.

Stack multiple parameters to create more specific filter expressions. 

Add multiple parameters to create more specific filter expressions.

## Review a Control

1. Select a specific control.

2. Review basic attributes. At the top of the right panel you can see:

   * **Control title**: Title of the Control

   * **Severity**: H, M, L

   * **Type**: **Cluster**, **Host**, **Identity**, **Resource**

   * **Author**: For example, Sysdig for out-of-the-box controls

   * **Description**: Meaningful description for the control.

   * **Policies**: The policies to which the control is linked.

     Hover over the policy names to get full details, such as the exact requirement number for the particular compliance standard, and to navigate to that requirement in the policy details page.


3. **Code:** Use the provided code snippets.

   The code provides visibility into the precise objects that are evaluated and how the evaluation rules are structured. The display includes **Inputs** (where applicable) and the evaluation code written in **Rego**. 


   You can copy and download the code as a `.json` file.

4. **Remediation Playbook:** Review the recommended steps in the Remediation Playbook to resolve failing controls. Sometimes, you must provide the applicable input in the provided remediation code.

   ![](/image/cspm_remed.png)

5. **Configure Parameters:** Edit the assigned **Severity** of a control. For some controls, edit other evaluation parameter values.

## Customize Posture Control

Sysdig incrementally adds the ability to customize posture control by adding parameters defined within posture controls.

### Configure Severity

By default, Sysdig assigns an opinionated severity level to the posture controls it delivers.

To edit the Severity assignment:

1. Select a specific control and select the **Parameters** tab.

2. Click **Customize**.

    <div style="max-width: 70%">
       <img src="/image/control_params2.png" />
     </div>


3. Choose the **Severity** level, and click **Save**.

   A success message appears. The Compliance results will be updated during the next evaluation. 
   
{{% callout type="note" %}}

You can duplicate a control and assign two different severities if it suits your use case. 

{{% /callout %}}

### Configure Evaluation Parameters

1. Select a specific control and select the **Parameters** tab.

2. Click **Customize**.

3. Customize an evaluation parameter.

  <div style="max-width: 70%">
     <img src="/image/control_params3.png" />
   </div>


4. Click **Save**.

### Customize Labels

Currently, custom **labels** can be added or deleted  from controls if labels were present in the original control.

1. Select a control that contains labels and open the **Parameters** tab.

2. Click **Customize**.

3. Enter a label in  **key:value** or  **key** format, such as `env:prod` or `owner`.

4. Click **Return**.

   If you want to add another label, insert a comma and the next label and click **Return** again.

5. Click **Save** when your list is complete.

The labels are displayed as Custom Values in the Parameters tab.

![](/image/control_remed.png)


{{% callout type="note" %}}

To include custom labels in the  Remediation Playbook YAML, you must download the sample code and enter the created labels manually.


{{% /callout %}}

## Custom Controls

You can now duplicate and edit any control. For example, you can create a new custom control that checks for a specific label.

### Guidelines

- You cannot link a user-duplicated control to an out-of-the-box policy.
- You can use a control from one policy in another custom policy with different parameter values. Even a change in severity level constitutes a different use case.
- You can delete or edit a duplicated (custom) control.
- Best practice: Establish naming conventions to make searching for custom controls in the Control List easy.

### Duplicate a Control

1. Select **Policies** > **Posture** > **Controls** and browse or search for a specific control.

   **Example:** If a control contains labels, the default Control Name includes the word **Label**. Search on `label` to find these controls and duplicate them if desired.

2. Right-click the three-dot menu and select **Duplicate**.

3. Edit the following as desired and click **Save**:

   **Name:** By default, Sysdig creates the **Original Control Name (copy)**. You can enter any name that is not used by any other existing control.

   **Description:** Optionally, enter a meaningful description about the control.

   **Severity:** You can select a severity for your custom control.

4. If the original control contained configurable evaluation parameters, you can customize them.

   See [Configure Evaluation Parameters](#configure-evaluation-parameters).

5. You can now link the new custom control to a requirement in your custom policies.

### Edit or Delete Custom Controls

1. Select a custom control from the Control List.

   Tip: Look for the Author to identify custom controls. If the author is not `Sysdig`, then the control is custom.

2. **To edit:**  From the 3-dot menu, select **Edit**, change the Control Details (Name/Description/Severity) as needed, and click **Save**.

   The changed details will be applied to any custom policy where the control is used after the next evaluation.

3. **To delete:** From the 3-dot menu, select **Delete** and confirm after the warning, **Yes, Delete**.

   The Compliance and Posture results of evaluations made from this control and any accepted risks for the control are deleted upon the next evaluation.


### Customizable Default Controls

The following controls are now customizable and have evaluation parameters that could be configured to personalize posture controls for specific use cases:

#### AWS

- API Gateway - Gateway with VPC_LINK connection type
- API Gateway - REST API Gateway Stage with unencrypted cache
- API Gateway - Rest API Gateway Stage without required tag
- API Gateway - Rest API Gateway without required tag
- API Gateway - Stage without required ACL
- AutoScaling - App-Tier Auto Scaling Group with associated Elastic Load Balancer
- AutoScaling - Auto Scaling Group Cooldown Period
- CloudFront - Use CloudFront Content Distribution Network
- ECR - Restricted User Access
- ElastiCache - Valid Node Type
- ElastiCache - Latest MemCached Engine Version
- ElastiCache - Latest Redis Engine Version'
- ElastiCache - Latest Instance Generation
- ElastiCache - Appropriate Cluster Cache Nodes Count
- ElastiCache - Expiration of Lease for Reserved Cache Node
- ElastiCache - Recent Acquisitions of Reserved Cache Nodes
- IAM - Appropriate Access Key Rotation
- IAM - Appropriate Password Minimum Length
- IAM - Appropriate Password Reuse
- IAM - No Unused Passwords
- IAM - No Unused Access Keys
- IAM - Unused Root Account
- Lambda - Function Uses Supported Runtime
- Lambda - Lambda Cross Account Access
- Networking - Defined Compliant destination-cidr-block in Routing Tables
- Resource Contains Required Labels
- SNS - Appropriate Subscribers
- SNS - Cross Account Access
- S3 - Enabled Encryption At Rest

#### GCP

- GCR - Restricted User Access
- Project - Corporate Credentials

#### Azure

- ACR - Restricted User Access
- AppService - Enabled Java Autoupdate
- AppService - Required Latest PHP Version
- AppService - Required Latest Python Version
- AppService - Required Latest TLS Version
- Compute - Installed Endpoint Protection
- Logging - Appropriate Diagnostic Setting
- Networking - Appropriate Flow Retention Setting
- PostgreSQL - Appropriate Log Retention Setting
- SQL Server - Appropriate Auditing Retention

#### Kubernetes

- ACR - Approved Registries
- API Server - Access to Pod Spec (OCP4)
- API Server - Defined audit-log-maxage
- API Server - Defined audit-log-maxbackup
- API Server - Defined audit-log-maxsize
- API Server - Owner of Pod Spec (OCP4)
- Approved Registries
- Container Contains Required Labels (new control)
- Container with Forbidden Capabilities (new control)
- GCR - Approved Registries
- Kubelet - Appropriate event-qps Level
