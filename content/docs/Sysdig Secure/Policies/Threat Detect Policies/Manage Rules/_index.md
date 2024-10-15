---
linkTitle: "Manage Threat Detection Rules"
title: "Manage Threat Detection Rules"
weight: "11"
aliases:
  - /en/manage-rules.html
  - /en/manage-threat-detection-rules/
  - /en/docs/sysdig-secure/policies/manage-rules/
  - /en/manage-rules
description: "Rules are the fundamental building blocks you use to compose your security policies. This page guides you through the rule library, and how to create and manage rules."
---

## Access the Rules Library

To access the Rules Library from Sysdig Secure, select **Policies** > **Rules** > **Rules Library**.

The Rules Library appears.

![](/image/rules_library.png)

### Navigate the UI

- Groups: Rules are grouped by source and you can collapse or open the groupings as needed. 

-   **Search:** Click the magnifying glass if the Search field is not automatically opened. Search by words in the rule name.

-   **Published by:** Default (Falco) rules appear as `Published by: Sysdig` ; user-created rules show as `Published by: Secure UI`.

-   **Last Updated:** Sort by the date the rule was added or updated. Badges highlight rules that were added or changed in the past seven days.  
    
-   **Usage:** Shows the number of policies where the rule is used, and whether the policies are enabled. Click the rule to see the policy names in the Rule Detail panel.
    
- **Tags:** Click the colored tag boxes to filter by tags.

### Available Filters

Use the various filters to find rules: 

* **Select Usage:** Choose rules that have been **Enabled**, **Disabled**, or **Not Used** in a policy
* **Select Tags:** Filter items by tag by typing them into the **Select Tags** field, choosing them from the drop-down, or clicking the colored tag boxes from the rules list.
* **Select Rule Source:** In addition to being grouped by source, you can also filter down to show only particular sources.
* **Managed Type:** Choose **Managed Rule** (from Sysdig) or **Custom Rule**.
* **Review Changes:** Filter to find rules that have been changed in the past seven days. Filter by :
  * **All changed rules:** includes all types of changes in the past seven days
  * **Changes by Sysdig:** includes rules automatically updated by Sysdig 
  *  **Changes by Tuner:** includes changes made by the auto-tuner
  *  **Changes by user:**  includes user-initiated changes submitted via the Exceptions UI, the Rules Editor YAML file, or by API

## Review a Rule Detail Panel

From the Rules Library list, select a rule to see its details panel.

![](/image/rule_workload.png)

From here you can:

-   Review the rule definition, including clicking embedded macros to open their details in a pop-up window.
-   Check all policies in which the rule is used and see whether those policies are enabled or disabled.
-   Check changes made to a rule.

## View Recent Changes to a Rule

When rules are changed, either by the Sysdig Threat Detection team or by users, an **Updated** badge is displayed next to the rule name for seven days. 

There are several ways to view recent changes to a rule. 

### From the Rules Library

1. Use the **Review changes** filter to find a list of recently changed rules. From the drop-down, you can find the rules changed by Sysdig, Tuner, or you.

2. Open the detail panel and select the **+/-** rule diff icon to compare the two versions of the rule. 

   ![](/image/rule_change.png)

3. You can select another row without closing the rule diff window to see changes to that rule. 

## Add Rules

You can select existing rules from the Library or create new rules on the fly and add them to a policy.

The Policy Editor interface provides many flexible ways to add rules to or remove rules from a Policy - the following steps describe one of these methods.

### Import from Library

1. From Sysdig Secure, click **Policies** > **Threat Detection** > **Runtime Policies**.

2. Click **Add Policy** in the top right corner, and select the policy type.

   Do not choose **Container Drift**, **Machine Learning**, or **AWS ML**, as these types do not support imports from the rules library.

   The **New Policy** page opens.

3. From the **New Policy** (or Edit Policy) page, click **Import from Library**.

   The **Import from Rules Library** page appears.

4. Select the checkboxes by the rules to import.

{{% callout type="tip" %}}

You can pre-sort a collection of rules by searching for particular keywords or tags, or clicking a colored tag icon.

{{% /callout %}}

3. Click **Mark for Import**.

4. Click **Import Rules**.

   The Policy page with the selected rules listed appears.

{{% callout type="tip" %}}

You can remove a rule from a Policy by clicking the X next to the rule in the list.

{{% /callout %}}

5. Fill out the remaining fields and **Save** your policy.

### Create a List-Matching Rule: Container Type Example

Suppose you want detect whenever someone uses a specific container image that has known problems. In this case, a Container rule would be appropriate. To create this rule:

1. Log in to Sysdig Secure, and navigate to **Policies** > **Rules** > **Rules Library**.

2. From the **Rules Library** page, click **+Add Rule** and select **Container** from the drop-down.

   The **New Rule** page for the Container rule type is displayed.

3. Enter the parameters:

   **Name:** Enter a name, such as "Problematic Images".

   **Description:** Enter a Description, for example, "Images that shouldn't be used".

   **If Matching/ If Not Matching:** Select **If Matching**. When added to a policy, if the rule conditions match, then the policy action you define (such as "send notification") is triggered.

   **Containers:** Add the container names that are problematic, for example: `cassandra:3.0.23`.

   **Tags:** Select relevant tags from the dropdown, for example: **database** and **container**.

4. Click **Save**.

The other list-matching rule types have similar entry fields, as appropriate to their type.

## Edit a Rule

Any rules published by Sysdig are default and read-only. You can append their lists and macros, but cannot change the core parameters or delete them.

You can freely edit any rules you create. You can also use a `placeholder` mechanism in the Rules Editor to override the behavior of default Falco rules and macros.

To display existing rules:

1.  Select **Policies** > **Rules** > **Rules Library** and select a rule.

2.  The Rule Details panel opens on the right. You can review the parameters and [append to macros and lists](#append-to-falco-macros-and-lists) inline.

    <img src="/image/rule_db.png" width="700" />

    The Rule `DB programed spawned process`, with the Rule Detail panel opened on the right.

### Append to Falco Macros and Lists

The default Falco rules have a variety of macros and lists embedded in them. You cannot delete these from a default rule, but can append additional information to them.

For example, consider the Policy `DB Program Spawned Process` in the image above. The embedded rule is used to check that databases have not spawned illicit processes. You can see the rule condition in the  : `db_server_binaries` Falco list.

{{% callout type="note" %}}

You can append to the condition and output of a managed rule, but you cannot append to tags, description, and rule type.

{{% /callout %}}

#### Append Items to the List

1.  Click the text in the rule condition, or go to **Policies** > **Rules** > **Falco Lists** and search for it by name.

2.  The list content appears. Click **Edit Append**.

    <img src="/image/rule_db_condition.png" width="500" />

3.  Enter the additional items (for example, databases) you want to include in the rule and click **Save**.

#### Append Conditions to the Macro

1.  Click the blue macro text in the rule condition, or go to **Policies > Falco Macros** and search for it by name.

    <img src="/image/macros_list.png" width="500" /> 

2.  The macro content appears. Click **Append**.

    <img src="/image/macro_append.png" width="500" />
    
3.  Enter the additional conditions you want to include with prepending logical `or` or `and` operators and click **Save**.

    <img src="/image/macro_save.png" width="500" />

### How to Use the Rules Editor

The Rules Editor lets you freely create custom Falco rules, lists,
and macros and can override the behavior of the defaults.

#### Understand the Interface

To access the interface, select **Policies** > **Rules Editor**:

![](/image/rules_editor.png)

**The Right Panel (Default)**

Displays the `rules_yamls` provided by Sysdig.

-   Contains the default rules and macros

-   Is read-only

**The Left Panel (Custom)**

Displays the custom rules and overrides you want to add to the selected
`rules_yaml`.

Note that many default Falco rules and macros have a parallel `placeholder` entry (commented out) in the YAML file. These have the prefix `user_known`. To change the behavior of a default rule, copy the placeholder equivalent into the custom rules panel and edit it there, rather than editing the default rule directly.

**To search the rules YAML files**

Click inside the Rules Editor right panel and use `CTRL + F` (in Windows. Use `Command + F` in Mac) to open an internal search field.

## Rule Exceptions

To reduce false positives, Sysdig uses Falco exceptions in many of the default rules, including adding exceptions to community rules. Rule exceptions are used as part of Runtime Policy Tuning.

To understand how rule exceptions in Sysdig build upon and differ from Falco, see [About Rule Exceptions](/en/docs/sysdig-secure/policies/threat-detect-policies/#about-rule-exceptions). You can manage rule exceptions in the Rules Editor UI or using YAML. 

### Manage Rule Exceptions with the UI

The exception builder gives users an easy way to apply or add new exceptions to managed or custom rules.

#### Modify an Existing Exception

For managed rules, you:

* Cannot modify or remove default exceptions
* Can append new exceptions
* Modify any appended exceptions 

After you define an exception with the **fields** (`container.name`) and **comparisons** (`endswith`) you cannot change, add, or remove it. 

![](/image/exceptions-ui-modify.gif)

#### Create a Custom Exception

For managed and custom rules: 

* You can create new exceptions.
* Just as with any of the exceptions, once you have applied an exception to a rule, you can only modify the values fields. You cannot modify or add to the fields or comps. 

![](/image/exceptions-ui-create.png)

#### Apply Exceptions

After each creation, deletion, or modification of an exception, select the **Apply** button to verify the action.
![](/image/exceptions-ui-delete.gif)

### Manage Rule Exceptions with YAML
#### Define a New Exception 

You can define a custom exception to configure optional values. This is useful when the values are not yet known. It also provides the policy tuner with fields to suggest new values for.

```
- rule: my custom rule
...
  exceptions:
    - name: cmdline_writer
      fields: [proc.cmdline, fd.directory]
      comps: [startswith, =]
```

#### Append Values to an Exception
You can append values to the exception by specifying the rule name and the exception name, without redefining the entire exception.

```
- rule: my custom rule
...
  exceptions:
    - name: cmdline_writer
      values: [httpd, /etc/shadow]
  append: true
```

After appending, the rule with the full exception now reads as:

```
- rule: my custom rule
...
  exceptions:
    - name: cmdline_writer
      fields: [proc.cmdline, fd.directory]
      comps: [startswith, =]
      values: [httpd, /etc/shadow]
```



## Use Cases: List-Matching Rules

It is more helpful to think of the rules as matching the activity, rather than using concepts of allowing or denying. The use cases are based on answering the question: What do I want to know?

**I WANT TO KNOW...**

**when any process other than web server programs are run:**

-   Rule Type: `Process`

-   `If Not Matching`

-   Entries: `[apache, httpd, nginx]`

**if any of the following crypto-mining processes are run:**

-   Rule Type: `Process`

-   `If Matching`

-   Entries:` [minerd, ccminer]`

**if any program reads any file containing password-related
information:**

-   Rule Type: Filesystem

-   Read Operations: `If Matching`

-   Entries:
    `/etc/shadow, /etc/sudoers, /etc/pam.conf, /etc/security/pwquality.conf`

**if any program writes anywhere below binary directories:**

-   Rule Type: `Filesystem`

-   Read/Write Operations: `If Matching`

-   Entries:` /usr, /usr/bin, /bin`

**if a program writes to anywhere other than /var/tmp:**

-   Rule Type: `Filesystem`

-   Read/Write Operations: `If Not Matching`

-   Entries: `/var/tmp`

**if any container with an image from docker.io is started:**

-   Rule Type: `Container`

-   `If Matching`

-   Entries: `[docker.io/]`

**if any container runs an Apache web server:**

-   Rule Type: `Container`

-   `If Matching`

-   Entries: `[httpd, amd64/httpd]`

**I want to know if any container with a non-database image is started:**

-   Rule Type: `Container`

-   `If Not Matching`

-   Entries `[percona/percona-server, mysql, postgres]`

**if any program accepts an inbound ssh connection:**

-   Rule Type: Network

-   Tcp,` "If Matching"`

-   Entries: `[22]`

**if any program receives a DNS datagram:**

-   Rule Type: Network

-   UDP, `"If Matching"`

-   Entries: `[53]`

**if any program accepts a connection on a port other than http/https**

-   Rule Type: Network

-   TCP, `"If Not Matching"`

-   Entries: `[80, 443]`

**if any program accepts any inbound connection:**

-   Rule Type: `Network`

-   Inbound Connection: `Deny`

**if any program makes any outbound connection**

-   Rule Type: `Network`

-   Outbound Connection: `Deny`