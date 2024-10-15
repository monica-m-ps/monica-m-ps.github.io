---
linkTitle: "Risk"
Title: "Risk"
weight: "5"
aliases:
  - /en/risks/
description: "Sysdig Risks module consolidates findings from all the Cloud Native Application Protection Platform (CNAPP) focal areas, including runtime events, vulnerabilities, posture, and identity, while adding attack path analysis and prioritization."
---

{{% callout type="note" %}}

This feature is in Technical Preview.

{{% /callout %}}

## All Risks

The information Sysdig gathers falls into various product areas, but these don't exist in silos. The Risks page highlights Sysdig's ability to correlate these findings and bring them together to help you understand and prioritize the greatest risks in your environment. You can leverage these insights to make well-informed and strategic security decisions within the ever-changing landscape of the cloud environment.

![](/image/risks_landing3.png)

### Supported Data Sources

The Risks module presents findings from the following connected data sources:

* Kubernetes
* AWS Cloud Accounts
* Microsoft Azure

### Permissions

By default, Sysdig SaaS users assigned to the following roles are granted **READ** access to the Risks page:

* Standard User
* Advanced User
* Team Manager
* Service Manager

If you have custom user roles that should be given read access, or if you want to define a group of users who should not have access to the Risk page, you must create a custom role and assign **Risks** permission to it.

<div style="max-width: 50%">
   <img src="/image/risks_role.png" />
 </div><br>

For more information, see [Custom Roles](/en/custom-role/).

### Finding Category

 **Finding Categories** refer to the different types of security issues that contribute to a risk.  Current categories include:

* **Vulnerability**: A vulnerability was detected in a package or image.
* **Detection**: A high-confidence event was detected from a Sysdig Threat Intelligence or Threat Detection policy. These are managed by the Sysdig Threat Research team, contain only high-severity rules, and have a low level of false positives.
* **Insecure Configuration**:  A Posture control failed in a Compliance check.
* **Insecure Identity**: Identity and Access (CIEM)  permissions irregularities were detected.
* **Risk Factors** : Represents the findings which are aspects of the resource that can increase its risk.

### Risk Policies

Out of the box, Sysdig delivers a range of finding combinations called "policies". These policies generate different Risks. Each policy is built of necessary findings that must be matched and optional findings that add to the Risk.  Each unique combination of findings, necessary or optional, generates a different Risk.

| Policy                                                       | Description                                                  | Necessary Finding Types                                      | Affected Resource Type | Optional Findings                                            |
| :----------------------------------------------------------- | :----------------------------------------------------------- | ------------------------------------------------------------ | ---------------------- | :----------------------------------------------------------- |
| **Exposed Workload with Critical Vulnerability**             | This workload is publicly exposed and was found to be vulnerable to CVEs of Critical severity. | <ul><li>Publicly Exposed</li><li>Critical Vulnerability</li></ul> | Workload               | <ul><li>In Use</li><li>Has Exploit</li><li> High Confidence Event  </li><li>Privilege Control Failure</li></ul> |
| **Publicly Exposed EC2 with Critical Vulnerability**         | This EC2 is publicly exposed and was found to be vulnerable to CVEs of Critical severity. | <ul><li>Publicly Exposed</li><li>Critical Vulnerability</li></ul> | AWS EC2                | <ul><li>Has Exploit</li><li> High Confidence Event  </li></ul> |
| **Publicly Exposed S3**                                      | This S3 Bucket is publicly exposed.                          | Publicly Exposed                                             | AWS S3                 | <ul><li> S3 Accepts HTTP</li><li>* S3 Versioning Disabled</li><li> High Confidence Event  </li><li>Accessed by Suspicious IP</li></ul> |
| **Data Exfiltration Risk in 1 Hop**                          | This publicly exposed EC2 instance has access to S3 Buckets through an attached IAM Role or S3 Bucket policy. This poses a Data Exfiltration risk. | <ul><li>Publicly Exposed</li><li>EC2 Has Access to S3 via IAM Role or attached bucket policy</li></ul> | AWS EC2                | <ul><li>Critical Vulnerability</li><li> High Confidence Event  </li><li>Accessed by Suspicious IP</li></ul> |
| **Publicly exposed Azure VM with Critical Vulnerability**    | The Azure VM is publicly exposed and was found to be vulnerable to CVEs of Critical severity. | <ul><li>Publicly Exposed</li><li>Critical Vulnerability</li></ul> | Azure VM               | <ul><li>Critical Vulnerability</li><li>Has Exploit</li><li>Has Fix</li></ul> |
| **Publicly exposed Azure Blob**                              | This Azure Blob Container is publicly exposed.               | <ul><li>Publicly Exposed</li></ul>                           | Azure VM               | High Confidence Event                                        |
| **Exposed Workload Contains AI Package** <br> Requires Cluster Shield installed for this policy to work. | This Workload that contains AI Packages that is Ingress Exposed and may be exposed to the internet. AI workloads are a prime target of attack for threat actors because they contain access to your most sensitive data and the AI models are complex and resource-intensive | <ul><li>Exposed</li><li>Contains AI Package</li></ul>        | Workload               | <ul><li>High Confidence Event</li><li>Critical Vulnerability</li><li>In Use</li><li>Has Exploit </li><li>Privilege Control Failure</li></ul> |
| **Risky AWS User**                                          | The IAM user has admin access without MFA enabled and unrotated access keys. Proper identity security hygiene requires enabling MFA for all users and rotating access keys at least every 90 days. It is also recommended to review which permissions are actually utilized and use Sysdig’s Policy Optimization to achieve the least permissive access. | <ul><li>No MFA</li><li>Admin</li><li>Access Keys Not Rotated </li></ul> | AWS IAM User                | <ul><li>Suspicious AWS Console Login </li><li>Persistence Event</li><li>Lateral Movement Event</li><li>Privilege Escalation Event</li><li>High Confidence Event</li><li>Inactive</li></ul> |
| **Risky AWS Role**                                           | This IAM role has admin access and unused permissions deemed to be of critical severity by the Sysdig Threat Research Team. It is recommended to review which permissions are actually utilized and use Sysdig’s Policy Optimization to achieve the least permissive access. | <ul><li>Admin</li><li>Critical Unused Permissions</li></ul>  | AWS IAM Role                | <ul><li>Risky IAM Permissions</li><li>Risky S3 Permissions</li><li>Risky EC2 Permissions</li><li>Risky Lambda Permissions</li><li>Risky RDS Permissions</li><li>Inactive</li></ul> |
| **Potentially Compromised User**                             | This IAM Identity has potentially been compromised and should be investigated further. | <ul><li>Potentially Compromised</li></ul> | AWS User               | <ul><li>Anomalous AWS Console Login</li><li>Confirmed Compromised</li><li>Critical Permissions</li><li>Critical Unused Permissions</li></ul> |

Policies assess data coming from CSPM, KSPM, Cloud Log Ingestion, CIEM, Vulnerability Management, and Agent-Based Threat Detection. Risk policies require some or all of these components to be deployed to be evaluated. For example:

| Type of Resource or Finding              | Sysdig Feature                                         | Installation                                |
|------------------------------------------|--------------------------------------------------------|---------------------------------------------|
| Kubernetes clusters                      | KSPM (Compliance)                                      | [KSPM](/en/install-secure/#kubernetes-kspm) |
| AWS cloud resources                      | CSPM (Cloud compliance)                                | [Agentless](/en/aws-agentless)              |
| AWS high-confidence event detection      | CSPM + CDR (Cloud compliance + Cloud threat detection) | [Agentless](/en/aws/add-new-features)       |
| Insecure Identity findings               | CSPM + CIEM (Cloud compliance + Identity and Access)   | [Agentless](/en/aws/add-new-features)       |
| Packages in runtime with vulnerabilities | Compliance + In Use                                    | [In Use](#in-use)                           |

### Access Risks

1. Log in to Sysdig Secure and click **Risks**.

2. Open a detected Risk and analyze the **Affected Resource**.

3. Selected the affected resource to open the detail drawer and take further action.

## Filter Risks

Use the filters at the top of the page to sort through the detected Risks.

#### Zone

The Risk page is filtered to reflect your Team Scopes, including **zones**.  Filter by detected zones using the drop-down.  For setup details, see [Zones](/en/zones/).

#### Finding Category

The different types of security issues that contribute to a risk. Current categories include:

* Vulnerability
* Detection
* Insecure configuration
* Insecure Identity
* Publicly exposed

#### Resource Category

**Resource Categories** currently include:

* **Compute**: This includes workloads and EC2 instances.
* **Storage**:  This includes S3 buckets.

#### Last Change

This tracks a specific risk on an affected resource over time.

Options include:

* **Risk Increased**

  The affected resource moved up to a more severe Related Risk. Detected more findings or the same number of findings, but the severity is increased.

* **Risk Decreased**

  The affected resource moved down to a less severe Related Risk. Detected fewer findings or the same number of findings but the severity is decreased.

* **Resource Moved**

  The affected resource moved to another Related Risk. Detected the same number of findings with same severity

* **New Resource**

  This affected resource showed up in the Risk within the past week.

    {{% callout type="note" %}}

   If the resource's risk increases, decreases, or is moved at any point, the **Last Change** column reflects the change and the **New** label will no longer be displayed on the affected resource row.

     {{% /callout %}}

* **Blank**  

  There has been no change. This column is blank if this affected resource does not match any of the other options.

#### Severity

Sysdig has established three tiers to gauge the **Severity** of identified issues.

**Tier 1: Severity Levels**

In Tier 1, risks are categorized based on their severity levels. A risk's severity matches the highest tier of one of its findings:

* **Critical:** is assigned when findings include a **high-confidence detection** or a **critical vulnerability** from an **In Use** package.
* **High:** is assigned when a **critical vulnerability** has an **exploit**.
* **Medium:** is assigned when **Compute** has access to storage, critical unused permissions exist, or an insecure storage configuration accepts HTTP.
* **Low:** is assigned if none of the above conditions are met.

**Tier 2: Prioritizing between Risks of the Same Severity**

In Tier 2, risks tagged **Live** are prioritized higher. This means they have an affected resource with a high-confidence event that occurred within the past three hours. The number of findings also plays a crucial role in prioritization. More "**+**" symbols in the findings indicate a higher priority.

**Tier 3: Prioritizing between Risks of Same Severity and Finding Count**

In Tier 3, risks are prioritized by the number of affected resources. The higher the number of impacted resources, the higher the priority for mitigation efforts.

#### In Use

The **In Use** designation highlights packages, which are executed at runtime and contain vulnerabilities.

#### Has Exploit

**Has Exploit** designates any vulnerabilities detected that have a known method of exploitation to carry out malicious actions.

#### Live

A high-confidence event was detected within the last 3 hours. A high-confidence event refers to an event that is detected by a Sysdig Threat Intelligence or Threat Detection policy.

#### New

A Risk is tagged as **New** if previously we had no affected resources match this risk, and now at least one does. A Risk is considered “New” for 7 days.

## Review the Affected Resource

Open a Risk to review the affected resource.

1. Review the findings for critical issues.

   <div style="max-width: 90%">
      <img src="/image/risks_zone.png" />
    </div><br>

2. Hover over zone findings to:

   * Get a list of the affected zones for that resource.

   * Use arrow links to the zone pages.

   * Copy zone information, for example, to paste in the Inventory filter bar.

3. Click an affected resource to open the detail drawer, including the Attack Path visualization and the Findings tab.

4. Optionally, you can change the status to **In Progress** to indicate that the finding is under review.

   The default status is **Open**. Only Sysdig can move the status to **Closed** when the system can no longer detect the issue.

### Review the Attack Path

1. Select an affected resource to open its detail drawer.

   The **Overview** tab of the drawer includes the top-level Risk summary and date, then the **Attack Path**, **Risk Details**, and **Resource Details** sections.

2. Select **Explore** to enlarge the Attack Path pane.

   You can use the using the **+** icon or the full-screen icons to explore the attack path.

3. Click on each icon for details and relevant mitigation steps.

   <div style="max-width: 80%">
      <img src="/image/risk_review.png" />
    </div><br>

#### Example

For example, in the image above, you can see:

* **Public Exposure:** The resource is exposed to the internet and details on the **Load Balancer** icon explain why.

  <div style="max-width: 50%">  
      <img src="/image/risk_lb.png" />
  </div><br>

* **Workload:** The workload has a large number of findings, organized by **Insecure Configurations**, **Vulnerabilities**, and **Events**.

  * Select an **Insecure Configuration** icon to see the explanation, and click **View More** to manage the Control as you would in the Compliance module.

    You can accept risks, apply code to update the control, create a pull request, and other posture-related actions.

    <div style="max-width: 70%">  
        <img src="/image/risks_insecure.png" />
    </div><br>

  * Select one of the **Vulnerabilities** icons to see the explanation, and click **View More** to review the full package and CVE details and to manage as you would in the Vulnerabilities module.

    <div style="max-width: 70%">  
        <img src="/image/risks_vuln.png" />
    </div><br>
  
  * Select an **Event** icon to see the explanation and click **View More** to review the full details and manage as you would in the Runtime Events module.
  
    <div style="max-width: 70%">
       <img src="/image/risks_event.png" />
    </div>

### Review the Findings

The **Findings** tab helps you understand all the resources involved in a specific Risk and the findings on them. Sysdig highlights the highest-impact findings and suggests fixes to reduce the most risk with the least effort.

1. Select an affected resource to open its detail drawer and select the **Findings** tab.

   <div style="max-width: 50%">  
       <img src="/image/risk_findings.png" />
   </div><br>

2. Review **All Resources in the Risk Path**.

3. Review **Findings by Resource Type**.

   This provides a comprehensive list of all the findings associated with this Risk on all the resources in the Risk path, divided by Resource Types.

   Select any row  to open a drawer showing remediation details.

4. Review **Suggested Fixes to Reduce Risk**.

   These are the most impactful fixes suggested by Sysdig.

   ![](/image/risks_suggested_fix.png)

## Risks Terminology

| Term                  | Definition                                                   |
| --------------------- | ------------------------------------------------------------ |
| Finding               | A resource and its condition or behavior. For example, a cloud resource and its misconfiguration. A container with a vulnerability. |
| Finding Categories    | The different types of security issues that contribute to a risk. Current categories include: <br /> <ul><li>Vulnerability </li><li>Detection </li><li> Insecure Configuration </li><li>Insecure Identity </li><li> Publicly exposed</li></ul> |
| Finding Type          | An instance of a finding category. <br />For example: <br /> Finding Category = Insecure Config <br /> Finding Types = Failed control “S3 bucket accepts HTTP”, failed control “EC2 is permissive”<br /> Finding Category = Insecure Identity<br /> Finding Types = Role with unused critical permissions, User without MFA |
| High-Confidence Event | An event detected from a Sysdig Threat Intelligence or Threat Detection policy. These are managed by the Sysdig Threat Research team, contain only high-severity rules, and have a low level of false positives. |
| Live                  | A high-confidence event was detected within the last 3 hours. |
| Risk Policy           | A policy is composed of the required finding types and optional finding types found on a specific resource type. A "risk policy" is the basic rule from which Risks and their affected resources are created. <br />NOTE: These are delivered from Sysdig and are not accessed or edited by users, unlike other types of Sysdig Policies |
| Severity              | Significance of a risk, expressed in terms of the combination of consequences to the business and the likelihood of those consequences. |
| Zones                 | A business group of resources, defined by a collection of scopes of various resource types. For example: `Dev`- all my development resources. |

## Use Case: Compromised User

Overly permissive credentials can lead to lateral movement and privilege escalation, resulting in cloud breaches. The Risks module helps prevent attacks by detecting anomalous or suspicious user actions and enabling real-time incident response. You can use the **Potentially Compromised User** identity finding to correlate user activities to cloud breaches.

* **Potentially Compromised User**: Misconfigured identities and secrets, combined with certain operation patterns, often indicate a breach. Sysdig flags these as potentially compromised, helping you start investigating promptly. You can then determine to mark it as Compromised User. **Potentially Compromised** is the finding that is triggered when Sysdig detects anomalous or suspicious user actions. It indicates that you should  investigate this user.

  See [Risk Policies](#risk-policies) to understand the findings and impacted resources.

* **Compromised User**:  You have the capability to flag a user as Confirmed Compromised and it serves as a clear signal that the incident has been thoroughly triaged and is not a false positive. You can then take appropriate actions, such as deleting access keys, or deactivating or deleting the user.

Sysdig provides recommendations for each option, guiding you through the necessary steps to address the compromise. You see these recommendations when a user is flagged as **Potentially Compromised**, and they remain the same even if you mark the user as Compromised.

Additionally, you can also view  potentially compromised users in the [Identity](/en/docs/sysdig-secure/identity/users/optimize-aws-users/#use-case-potentially-compromised-user) module.

### Correlate Risk Findings to Identities

When a potential identity breach occurs, Sysdig Risks page shows the detected risk. Click the **Compromised User** to see affected resources.

<div style="max-width: 60%">
   <img src="/image/compromised-user-risk.png" />
 </div>

Additionally, you can also view incidents related to potentially compromised users in the [Events Feeds](/en/docs/sysdig-secure/identity/users/optimize-aws-users/#use-case-potentially-compromised-user).

### Triage the Affected Resources

You can then triage the event as follows:

1. Click one of the resources to open the Risk Details panel.

2. Click the **Findings** tab.

   You can see all the resources in the risk path and findings by resource type. Clicking **Resources** takes you to the Inventory page.

3. Click one of the **IAM User Findings** or the user under **Suggested Fixes to Reduce Risk**.

   <div style="max-width: 70%">
      <img src="/image/risk-findings-compromised-user.png" />
    </div>

   The Policy Details page appears. On this page, you can review the policy, rules, AWS user details, cloud account details, IP address, and API calls to gain insight into the affected resource.

4. Click on an Event finding in the **IAM User Findings** section.

5. On the Event Details panel, click **Investigate**.

   <div style="max-width: 70%">
      <img src="/image/breached-user.png" />
    </div>

   You will directed to the **Identity** page, where you can see the cloud account, IP address, AWS region, user in question, and the user activities.

6. Click the user to:

   * **Summary**: View a summary of unused permissions, total permissions, criticality of permissions and unused permissions. The critically is determined by the excessive permission granted to the user through the roles attached to the User. The details panel also shows when the user was last active, User ARN, and account ID.

     The Summary tab also shows that if the user is **Compromised** or **Potential Compromised**.

     Sysdig does not mark a user as **Compromised**. You should manually check the Potentially Compromised User, triage the activities, and mark it appropriately.

   * **Respond**: Mark as **Compromised** if you think the user is compromised.

     * Mark as **Compromise Resolved** if you have already taken remediation actions.

   * **Remediation Strategies**: Provides two high level strategies to mitigate the risk.

     * **Contain Compromise**: You can address the compromise in the following ways:
       * Add Restrictive Policy for IP
       * Deactivate User
       * Delete User
       * Force Password Reset
       * Delete & Create New Access Keys
     * **Consolidate and Reduce Permissions** : Create a new custom IAM Policy for the User with a subset of only used permissions.

7. Select a strategy you prefer.

   For example, if you decide to delete the user, mouse over **Delete User** and click **View Remediation**.

   1. View the remediation instructions for both **AWS Management Console** and **AWS CLI**.

   2. Click **Open in Console** to open the AWS Management Console to perform the actions given in the remediation instructions.

      Optionally, you can perform the operations using the AWS CLI.

8. If you want to consolidate and reduce the permissions assigned to the user, click **View Remediation** button next to the **Create a new custom IAM Policy for this User with a subset of only used permissions** option.

   1. Review the IAM policies and instructions to create a new IAM policy.

   2. Click **Open in Console** to open the AWS Management Console to perform the actions given in the remediation instructions.

      <div style="max-width: 70%">
         <img src="/image/reduce-permissions.png" />
       </div>

9. Return to Sysdig Secure UI and mark the AWS IAM user as **Compromise Resolved**, as given in step 5.

   <div style="max-width: 70%">
      <img src="/image/respond-compromised.png" />
    </div>
