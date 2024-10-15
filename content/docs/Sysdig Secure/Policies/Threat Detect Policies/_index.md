---
linkTitle: "Threat Detection"
title: "Threat Detection Policies"
weight: "9"
no_list: true
aliases:
  - /en//docs/sysdig-secure/policies/threat-detect-policies/
  - /en/docs/sysdig-secure/sysdig-secure-for-cloud/aws/
  - /en//docs/sysdig-secure/sysdig-secure-for-cloud/aws/aws-cloudtrail-falco-rules/
  - /en/docs/sysdig-secure/policies/cloud-account-rules/aws-cloudtrail-falco-rules/
  - /en/aws-falco-rules
  - /en/cloud-account-rules
  - /en/docs/sysdig-secure/sysdig-secure-for-cloud/
  - /en/docs/sysdig-secure/policies/cloud-account-rules/
  - /en/docs/sysdig-secure/sysdig-secure-for-cloud/azure/azure-platformlogs-falco-rules/
  - /en/docs/sysdig-secure/sysdig-secure-for-cloud/azure/
  - /en/docs/sysdig-secure/policies/cloud-account-rules/azure-platformlogs-falco-rules/
  - /en/azure-platformlogs-falco-rules
  - /en/gcp-auditlog-falco-rules.html
  - /en/docs/sysdig-secure/sysdig-secure-for-cloud/gcp/
  - /en/docs/sysdig-secure/sysdig-secure-for-cloud/gcp/gcp-auditlog-falco-rules/
  - /en/docs/sysdig-secure/policies/cloud-account-rules/gcp-auditlog-falco-rules/
  - /en/gcp-audit-falco-rules
  - /en/threat-policies/
  - /en/threat-policy
description: "Sysdig Secure manages Runtime Threat Detection through policies. These policies consist of rules to detect and respond to suspicious activity in your environments. This page outlines the concepts to use Threat Detection Policies."
---

## Runtime Policies

Threat Detection Runtime Policies provide granular control over runtime behavior by defining rules and configurations. Policies specify where to apply rules and how to respond to security violations. Use policies to monitor clusters, hosts, or user-defined tags and take actions such as alerting Slack channels, triggering webhooks, or preventing malware from starting.

There are three types of policies, each with their own set of attributes:
- Managed Policies
- Managed Ruleset Policies
- Custom Policies

Additional types are added periodically.

<div style="max-width: 60%">
   <img src="/image/policy_type2.png" />
 </div>

### Managed Policies

Out of the box, Sysdig provides pre-built, expertly curated policies to detect and prevent various security threats. Created and maintained by Sysdig’s Threat Research Team, these policies provide comprehensive protection against malware, data exfiltration, intrusions, DDoS attacks, and more. They enable quick and effective threat detection and can be customized to meet the specific needs of your organization.

<div style="max-width: 60%">
   <img src="/image/managedp1.png" />
 </div>

#### Attributes

The default managed policies have the following attributes:

* They exist across all accounts, their names cannot be changed, and they cannot be deleted.

* They are loaded with a pre-defined enabled/disabled status, based on most common usage, but you can manually **Enable** or **Disable** them.

* You can only edit their **Scope** and **Action**.


#### Manage Daily Updates (On-Prem Only)

For on-premises deployments (v 6.1.1+), the managed policies and rules are updated daily at midnight UTC. The schedule is handled automatically by a cron job service `sysdigcloud-falco-rules-deployer`.

**To change the frequency:**

Edit `Values.sysdig.secure.rulesDeployer.schedule` to your preferred settings.

**To disable:**

Edit `Values.sysdig.secure.rulesDeployer.enabled`  to suspend the cron job.

### Managed Ruleset Policies

Duplicate a managed policy if you want to edit additional attributes.

<div style="max-width: 60%">
   <img src="/image/managedr1.png" />
 </div><br>

Example use case is when you need different scopes or actions, such as notification channels, for the same set of rules within a Managed Policy.

#### Attributes

Managed ruleset policies, duplicated from a parent managed policy, have the following attributes:

* You can edit the **Name**, **Description**, **Severity**, **Scope**, and **Action**.

* As with the default Managed policies, Managed Ruleset policies are updated by the Sysdig Threat Research team.


### Custom Policies

Custom policies can be created in three ways:

* Convert a Default managed policy to Custom
* Convert a Managed Ruleset policy to Custom
* Create a policy from scratch

Any policies from before July, 2022 are auto-converted to Custom policies and continue to work as they did before.

#### Attributes

Custom policies cannot be updated by the Sysdig Threat Research team. You must manually apply new rules to custom policies.

## Review the Runtime Policies List

Select **Policies** > **Threat Detection** | **Runtime Policies** to see the default policies loaded into Sysdig Secure, as well as any custom policies you have created.

From this overview, you can:

#### See at a Glance

* Collapse or expand policy groups: Policies are grouped by policy type.


-   **Severity:** Default policies are assigned **High**, **Medium**, **Low**, or **Info** level severity, which can be edited.
-   **Enabled/Not Enabled**: Viewed by toggle position.
-   **Policy Summary**: Includes **Update** status, the number of **Rules**, assigned **Actions** to take on affected containers (**Stop | Pause | Notify**), and **Capture** details, if any.
-   **Policy Status**: The **Default** policies are managed policies,  **Ruleset** are managed ruleset policies, and  **Custom** policies may be user-designed from scratch or converted from default policies with changes to their rules.
-   **Added/Updated Badges** indicate managed policies with rules that have been added or updated in the past 7 days.

{{% callout type="note" %}}
When you create a policy in the UI, you define the severity as, Info, Low, Medium, or High. From an API perspective these are mapped to a number from 0 to 7. When an event is triggered, the corresponding number to the severity is displayed:

|  Number  | Severity   |
| -------- | ---------- |
|      0   | High (UI Default) |
|      1   | High  |
|      2   | High  |
|      3   | High  |
|      4   | Medium (UI Default) |
|      5   | Low (UI Default)  |
|      6   | Info  |
|      7   | Info (UI Default) |

{{% /callout %}}

#### Take Action

From this panel, you can also:

-   Drill down to policy details, and potentially **Edit** them.

-   **Search** and **Filter** policies by name, severity level, type, or whether captures are enabled.

-   **Enable** or **Disable** a policy using the toggle.

-   **Create** a new policy by selecting **+Add Policy**.

## View Recent Changes

When rules are changed, either by the Sysdig Threat Detection team or by users, an **Updated** badge is displayed next to relevant policies  for seven days.

There are several ways to view recent changes to a rule.

### From Runtime Policies

1. Go to **Polices** > **Threat Detection** | **Runtime Policies** and scroll down to find any **Updated** badges.

2. Select the policy to open the detail panel and scroll to find the updated rule.

   <div style="max-width: 60%">
      <img src="/image/policy_update_rule.png" />
    </div>

3. Select the **+/-** rule diff icon to compare the two versions of the rule.

   <div style="max-width: 60%">
      <img src="/image/rule_diff.png" />
    </div>

### From the Policy Edit Page

1. Go to **Policies** > **Threat Detection** | **Runtime Policies** and select a policy.

2. Click the **Edit** (pencil) icon to open the Policy Edit page.

3. Select a rule from the page.

4. If the rule has been updated, you can use the **+/-** icon next to the rule.

   <div style="max-width: 65%">
      <img src="/image/policy_edit_diff.png" />
    </div><br>

   If the change has happened in the past seven days, there will also be an icon available next to the Updated badge on the main Policy Edit page.

See [View Recent Changes to a Rule](/en/docs/sysdig-secure/policies/threat-detect-policies/manage-rules/#view-recent-changes-to-a-rule) from the Rules Library.

### Runtime Policies

#### Workload Policy

Powered by the [Falco](https://falco.org/) engine, these provide a way to filter system calls using flexible
condition expressions. See the [Workload Policy example](/en/workload-policy).

#### List-Matching Policy

Policies using a simple matching or not-matching for containers, syscalls, processes, etc. See
[Understanding List Matching Rules](/en/docs/sysdig-secure/policies/threat-detect-policies/#understanding-list-matching-rules) for more context.

#### Drift Policy

Policy with a single rule that provides default drift detection and prevention.

See the [Drift Control policy example](/en/drift).

#### Workload ML

Policy leveraging Machine Learning to provide advanced detection capabilities.

See the [Workload ML policy example](/en/machine-learning-policy).

#### Kubernetes Audit Policy

 Powered by the Falco engine, these policies provide a way to filter Kubernetes audit logs using flexible condition expressions. To use the managed Kubernetes audit policies included out of the box, you must first [install/enable the audit logging feature](/en/install-audit-logging/).

#### Okta Audit Log Policy

Provide threat detection for Okta environments when the [Okta integration](/en/secure-okta) is enabled.

#### AWS CloudTrail Policy

 Provides a way to filter [AWS CloudTrail](https://aws.amazon.com/cloudtrail/) events using Falco-compatible condition expressions.

You must have an [AWS Cloud Account connected](/en/aws-secure/) to transmit your AWS CloudTrail events.

#### GCP Audit Log Policy

 Provides a way to filter [GCP audit logs](https://cloud.google.com/logging/docs/audit) using Falco-compatible condition expressions.

You must have an [GCP Cloud Account connected](/en/gcp-secure) to transmit your GCP audit log events.

#### Azure Platform Log Policy

Provides a way to filter [Azure platform logs](https://docs.microsoft.com/en-us/azure/azure-monitor/essentials/platform-logs-overview) using Falco-compatible condition expressions.

You must have an [Azure Cloud Account connected](/en/azure-secure) to transmit your Azure platform log events.

#### AWS Machine Learning (ML)

Provides a way to detect anomalous AWS Console login events.  

See the [AWS ML](/en/aws-ml) Policy example.

You must have an [AWS Cloud Account connected](/en/aws-secure/) to transmit your AWS CloudTrail events.

#### AWS GuardDuty

Provides a way to receive GuardDuty findings as additional events.

You must have an [AWS Cloud Account connected](/en/aws-secure/) to transmit your AWS GuardDuty findings.


### Scopes and Actions for Policy Types


The scopes and actions available differ by type:

|                    Type                 | Options                                                                                        | Action Options                                           |
| --------------------------------------- | -----------------------------------------------------------------------------------------------| -------------------------------------------------------- |
| **Workload**<br/>**List Matching**      | `Custom`<br/>`Hosts only`<br/>`Container only`                                                 | Container stop / pause / kill<br/>Capture<br/>Notification channel |
| **Drift**                               | `Custom`<br/>`Hosts only`<br/>`Container only`                                                 | Prevent <br /> Container stop / pause / kill<br/>Capture<br/>Notification channel                       |
| **Workload ML**                         | `kubernetes.cluster.name` <br/>`kubernetes.namespace.name`                                     | Notification channel                                     |
| **Kubernetes**                          | `kubernetes.cluster.name` <br/>`kubernetes.namespace.name`                                     | Notification channel                                     |
| **AWS Cloud**<br/>**AWS ML**<br/>**AWS GuardDuty**            | `aws.accountId`<br/>`aws.region`                                         | Notification channel                                     |
| **GCP**                                 | `gcp.projectid`<br/>`gcp.location`                                                             | Notification channel                                     |
| **Azure**                               | `azure.subscriptionId`<br /> `azure.tenantId`<br />`azure.location`<br />`azure.resourceGroup` | Notification channel                                     |
| **Microsoft Entra**                     | `azure.tenantId`                                                                               | Notification channel                                     |
| **Okta**<br/>**Okta ML**                | `okta.org`                                                                                     | Notification channel                                     |
| **GitHub**                              | `github.org`<br/>`github.repo`                                                                 | Notification channel                                     |

### Report Policy Actions in Kubernetes Events

When a `stop`, `pause`, or `kill` action is performed, Sysdig includes a message in Kubernetes events to explain that Sysdig acted on the container due to the [given] rule.  You will see this information when you `kubectl` `events` in your terminal, without requiring login to the Sysdig Secure UI.

![(image)](/image/k8s_message.png)

**Enablement**

This feature is included with **agent v.12.18+**. If you have deployed agent 12.18+ using Helm, the feature and its associated permissions are enabled by default. If you deploy agents manually, you must set Kubernetes role permissions. See [Agent Configuration Library](/en/configuration-library#report-actions-in-kubernetes-events) for details.

## Understand Threat Detection Rules

Rules are the fundamental building blocks you will use to compose your
security policies. A rule is any type of activity that an enterprise
would want to detect in its environment.

Rules can be expressed in two formats:

-   **Falco rules**
    [syntax](https://falco.org/docs/rules/supported-fields/), which can
    be complex and layered. All the *default* rules delivered by Sysdig
    are Falco rules, and users can also create their own Falco rules.

-   **List-matching rules** syntax, which is simply a list against which
    a `match/not match` condition is applied. All these rules are
    *user-defined*. They are grouped into five types: Container Image,
    File System, Network, Process, and Syscall.

### Understand the Rules Library

The Rules Library includes all created rules which can be referenced in policies. Out of the box, it provides a comprehensive runtime security library with container-specific rules (and predefined policies) developed by Sysdig's threat-research teams, Falco's open-source community rules, and international security benchmarks such as [CIS](https://www.cisecurity.org/cis-benchmarks/) or [MITRE
ATT&CK](https://attack.mitre.org/).

#### Audit-Friendly Features

In the Rules Library interface, you can see at a glance:

-   **Published By:** Who published the rule.

-   **Last Updated**: When the rule was last updated.

{{% callout type="note" %}}
- **Default rules** appear in the UI as **Published By: Sysdig**
- **User-defined rules** appear as **Published By: Secure UI**
{{% /callout %}}

#### Tags

Rules are categorized by tags, so you can group them by functionality,
security standard, target, or whatever schema makes sense for your
organization.

Various tags are predefined and can help you organize rules into logical
groups when creating or editing policies.

#### Search

Use the search boxes at the top to search by rule name or by tag.

### Using Falco within Sysdig Secure

#### What is Falco

Falco is an open-source intrusion detection and activity monitoring
project. Designed by Sysdig, the project has been donated to the Cloud
Native Computing Foundation, where it continues to be developed and
enhanced by the community. Sysdig Secure incorporates the **Falco Rules
Engine** as part of its Policy and Compliance modules.

Within the context of Sysdig Secure, most users will interact with Falco
primarily through writing or customizing the rules deployed in the
policies for their environment.

Falco rules consist of a *condition* under which an alert should be
generated and an *output string* to send with the alert.

##### Conditions

-   Falco rules use the [Sysdig filtering
    syntax](https://falco.org/docs/rules/supported-fields/).

    (Note that much of the rest of the Falco documentation describes
    installing and using it as a free-standing tool, which is not
    applicable to most Sysdig Secure users.)

-   Rule conditions are typically made up of *macros* and *lists.*

    -   **Macros** are simply rule condition snippets that can be
        re-used inside rules and other macros, providing a way to factor
        out and name common patterns.

    -   **Lists** are (surprise!) lists of items that can be included in
        rules, macros, or other lists. Unlike rules/macros, they can not
        be parsed as Sysdig filtering expressions.

Behind the scenes, the `falco_rules.yaml `file contains the raw code for
all the Falco rules in the environment, including Falco macros and
lists.

#### Anatomy of a Falco Rule

All Falco rules include the following base parameters:

-   **rule** **name:** default or user-assigned

-   **condition:** the command-line collection of fields and arguments
    used to create the rule

-   **output**

-   **source**

-   **description**

-   **tags:** for searching and sorting

-   **priority**

Select a rule from **Rules** > **Rules Library** to see or edit its underlying
structure. The same structure applies when creating a new Falco rule and
adding it to the library.

<div style="max-width: 70%">
   <img src="/image/policy_new_rule.png" />
 </div><br>

{{% callout type="note" %}}

Falco rules with the source `k8s_audit` need [Kubernetes Audit
logging](/en/docs/sysdig-secure/threats/forensics/kubernetes-audit-logging/) enabled for
conditions to be met.

{{% /callout %}}

#### About Falco Macros

Many of the Falco rules in the Rules Library contain Falco macros in
their `condition` code.

You can browse the Falco Macros list, examine a macro's underlying code,
or create your own macro. The default Falco rule set defines a number of
macros that make it easier to start writing rules. These macros provide
shortcuts for a number of common scenarios and can be used in any
user-defined rule sets.

<div style="max-width: 70%">
   <img src="/image/policy_falco_macro.png" />
 </div><br>

#### About Falco Lists

Default Falco lists are added to improve the user experience around
writing custom rules for the environment.

For example, the list `allow.inbound.source.domains` can be customized
and easily referenced within any rule.

#### About Rule Exceptions

To reduce false positives, Sysdig uses Falco [exceptions](https://falco.org/docs/rules/exceptions/) in many of the default rules, including adding exceptions to community rules. Rule exceptions are used in the auto-tuning and rules exception features as part of [Runtime Policy Tuning](/en/runtime-policy-tuning).

To understand how exceptions are managed: there are the exception definitions, used to define a set of fields and comparisons (comps) that the values (which are optional) can use to create those exceptions

#### Available Fields

Sysdig SaaS and the Sysdig Agent enrich events with details that are not available to Falco. The most common classes of fields available are:

##### Workload/syscall Rules

* `proc`
* `user`
* `group`
* `container`
  * See below for container fields that are not available from Falco
* `fd`
  To understand each field in those classes, you can find them [here](https://falco.org/docs/reference/rules/supported-fields/).

The Kubernetes fields are not available in the Falco rules. This is due to performance improvements that could affect the Kubernetes API server when collecting those values from Falco. In the event details you may see this information enriched from other parts of Sysdig, with values such as ``kubernetes.deployment.name``.

##### Kubernetes Audit/k8s_audit Rules

All `ka` fields are available. You can find a comprehensive list [here](https://github.com/falcosecurity/plugins/tree/master/plugins/k8saudit).

However as noted above, Sysdig enriches some additional metadata in the event details. An event may have the field `kubernetes.cluster.name`, however that is not available in the rule or rule exceptions.

##### Common fields that are not available

* `agent.tag.*`
* `kubernetes.*`
* `host.*`
* `container.label.*`
* `container.name.repo` instead use
  * `container.image.repository` which outputs `sysdig/agent`
  * `container.image` which outputs `sysdig/agent:latest`

#### (On-Prem Only) Upgrading Falco Rules with the Rules Installer

Sysdig Secure SaaS is always using the most up-to-date Falco rules set.

Sysdig Secure On-Prem accounts should upgrade their Falco rules set
regularly.

This can be achieved through our [Rules Installer](/en/docs/sysdig-secure/policies/install-falco-rules-on-premises/#install-falco-rules-on-premises).

### Understanding List-Matching Rules

List-matching rules (formerly known as "fast" rules) are used for
matching against lists of items (when `matchItems=true)` or matching
everything other than lists of items (when `matchItems=false`). They
provide for simple detections of processes, network connections, and
other operations. For example:

-   If this process is detected, trigger an action when this rule is in
    a policy (such as send notification).

    Or

-   If a network connection on x port is detected, trigger an action
    when this rule is in a policy (such as send notification)

Unlike Falco rules, the list-matching rule types do not permit complex
rule combinations, such as "If a connection on *x* port from *y* IP
address is detected..."

The five list-matching Rule Types are described below.

#### Container Rules

These rules are used to notify if a specific image name is running in an
environment. The rule is evaluated when the container is started. The
items in the list are image pattern names, which have the syntax
`<host.name>:<port>/<name>/<name2>:<tag>@<digest>`.

Only `<name2> `is required; everything else is optional and inferred
building on the name.

See also: [How Matching Works: Container
Example](/en/docs/sysdig-secure/policies/#how-matching-works-container-example)
and [Create a List-Matching Rule: Container Type
Example](/en/docs/sysdig-secure/policies/manage-rules/#create-a-list-matching-rule-container-type-example).

#### File System Rules

These rules are used to notify if there is write activity to a specific
directory/file. The rule is evaluated when a file is opened. The items
in the list are path prefixes.

For example: `/one/two/three` would match a path `/one/two/three`,
`/one/two/three/four`, but not `/one/two/three-four`.

#### Network Rules

These rules are used to:

-   Detect attempts to listen for inbound connections on ports on a
    specific list

-   Generally identify any inbound or outbound connection attempts

Note that the current Sysdig UI talks about "Allowing" or "Denying"
connections with network rules, but this can introduce some confusion.

For both Inbound and Outbound connections:

-   `Allow` means `do nothing`

-   `Deny` means
    `match any attempt to make an inbound or outbound a connection`

You would still need to add the rule to a policy and attach actions to
respond to a connection attempt by `stopping/pausing/killing `the
container where the connection occurred. See also: [Understanding How
Policy Actions Are
Triggered](/en/docs/sysdig-secure/policies/manage-policies/#understanding-how-policy-actions-are-triggered).

#### Process Rules

These rules are used to detect if a specific process, such as SSH, is
running in a particular area of the environment.

The rule is evaluated when a process is launched. The items in the list
are process names, subject to the 16-character limit enforced by the
Linux kernel. (See also: [Process Name Length
information](https://stackoverflow.com/questions/23534263/what-is-the-maximum-allowed-limit-on-the-length-of-a-process-name/23534499).)

#### Syscall Rules

{{% callout type="note" %}}

The `syscall` rule type is almost never deployed in user-created
policies; the definitions below are for information only.

{{% /callout %}}

These rules are used (internally) to:

-   Notify if a specific syscall happens in a list

-   Notify if a syscall outside this trusted list happens in the
    environment

The rule is evaluated on syscalls that create *inbound*
(`accept, recvfrom, recvmsg, listen`) and/or *outbound*
(`connect, sendto, sendmsg`) connections. The items in the list are port
numbers.

#### How Matching Works: Container Example

A Container Image consists of the following components:

`<registry host>:<registry port>/<image>:<tag>@<digest>`.

Note that `<image>` might consist of multiple path components such as
`<project>/<image>` or `<project>/<subproject>/<image>.`

**Complete example:**
`docker.io:1234/sysdig/agent:1.0@sha256:da39a3ee5e6b4b0d3255bfef95601890afd80709`

**Where:**

`<registry host>` = docker.io

`<registry port>` = 1234

`<image>` = sysdig/agent

`<tag>` = 1.0

`<digest>` = sha256:da39a3ee5e6b4b0d3255bfef95601890afd80709

Each item in the containers list is first broken into the above
components, using the following rules:

-   If the string ends in `/`, it is interpreted as a registry host and
    optional registry port, with no `image/tag/digest `provided.

-   Otherwise, it is interpreted as an image. The registry host and port
    may precede the image and are optional, and the tag and digest may
    follow the image, and are optional.

Once the item has been broken into components, they are considered a
prefix match against candidate image names.

**Examples:**

`docker.io:1234/sysdig/agent:1.0 @sha256:da39a3ee5e6b4b0d3255bfef95601890afd80709:`
must match all components exactly

`docker.io:1234/sysdig/agent:1.0: `must match the registry host, port,
image, and tag, with any digest

`docker.io:1234/sysdig/agent: `must match the registry host, port, and
image, with any tag or digest

`sysdig/agent: `must match the image, with any tag or digest. Would not
match an image `docker.io:1234/sysdig/agent`, as the image provides
additional information not in the match expression.

`docker.io:1234/:` matches all images for that registry host and port

`docker.io/:` matches all images for that registry host

## Getting Started

-   [Manage Policies](/en/docs/sysdig-secure/policies/manage-policies)

-   [Manage Rules](/en/docs/sysdig-secure/policies/manage-rules/#manage-rules)

{{% callout type="note" %}}
There are optional tools to help automate the creation of policies. See [Network Security Policy Tool](/en/docs/sysdig-secure/network/#network-security-policy-tool) for information on authoring and fine-tuning Kubernetes network policies.
{{% /callout %}}
