---
linkTitle: "Runtime Threat Detection Policy Tuning"
title: "Runtime Threat Detection Policy Tuning"
weight: "12"
aliases:
  - /en/runtime-policy-tuning.html
  - /en/docs/sysdig-secure/policies/runtime-policy-tuning/
  - /en/runtime-policy-tuning
  - /en/docs/sysdig-secure/policies/threat-detect-policies/runtime-policy-tuning/the-falco-rules-tuner-legacy/
description: "The Runtime Policy Tuning feature assists in reducing noisy false positives in the Sysdig Secure Events feed. It includes both an auto-tune feature and a tuning suggestion feature." 
---

When auto-tune is enabled, [exceptions](https://falco.org/docs/rules/exceptions/) are automatically added to Falco policy rules to remove noisy sets of events and leave lower-volume events for later analysis.

Additionally, event details (accessed either in Events feed or the Insights panels) may include Tunable Exception suggestions which can be ignored, applied, or edited and applied.

The tuner can be especially helpful when deploying Sysdig Secure runtime
policies in a new environment. Your environment may include applications
that legitimately perform actions such as running Docker clients in
containers, changing namespaces, or writing below binary directories,
but which trigger unwanted floods of related policy events in the
default policies and rules provided by Sysdig.

{{% callout type="note" %}}

Only users with the Admin user role are permitted to perform the actions in this section directly on the Tuner page.

{{% /callout %}}

## Use the Runtime Policy Tuner

There are two ways to use the policy tuner:

* **Auto-Tune:** [enable/disable](#enable-disable-auto-tuning)
* **Tunable Exceptions:** Accessed  in the [Secure Events feed](/en/events-feed#tunable-exceptions) and [Insights](/en/events-feed#tunable-exceptions), these features do not require the auto-tuning feature to be enabled.

### Enable/Disable Auto Tuning

You can enable and disable the tuner as needed to tame false positives and optimize the use of the Events feed. Auto tuning is enabled by default.

1. Log in to Sysdig Secure as Admin and choose **Policies** > **Threat Detection** > **Runtime Policy Tuning**.

2. Enable or disable the feature with the **Auto Tuning** toggle.

    It may take up to 24 hours for the initial Applied Tuning Exceptions to appear on the left panel.

    ![](/image/tuner_basic4.png)

    In the background, the tuner will evaluate policy events as they are received by the Sysdig backend, find applicable exceptions values, and add them. The Applied Tuning Exceptions file is passed along to all Sysdig agents, along with the [rules](/en/manage-rules) and [policies](/en/manage-policies).

3. Toggle the Tuning Engine off when you feel the feature has addressed the most commonly occurring or unnecessary policy events.

    To start over from scratch, clear the Applied Tuning Exceptions text and re-enable the Tuning Engine toggle.

{{% callout type="note" %}}

Turning the auto tuner off does not disable the exceptions that were added by the auto tuner, nor any exceptions manually added from the tuner suggestions.

{{% /callout %}}

### Edit & Review Exceptions

**Edit in Runtime Policy Tuning Page:** After an exception is added by either the auto tuning feature or tunable exceptions, admins can directly edit or review the exceptions in the editor file in the **Runtime Policy Tuning** page. If you are happy with the changes you have made, click **Save**. Otherwise, select **Discard Changes** to undo your edits.

**Review in the  [Rules Library](/en/docs/sysdig-secure/policies/threat-detect-policies/manage-rules/):** View rules that have been tuned by selecting those that have "**Published By: Tuner 0.0.0**"

![](/image/auto-tune.png)

## Understand How the Tuning Engine Works

### When Does the Tuner Add Exceptions?

The Policy Tuning feature is conservative, only adding exceptions for *commonly occurring events* from a *single rule* with *similar attributes*.

**All the conditions must be met:**

* The rule has generated at least 25 policy events in the past hour.

* A candidate set of exception values is applicable to at least 25% of
    the events in the past hour.

This ensures the tuning feature only adds exceptions for high-volume
sets of events that can be easily addressed with a single set of exception values.

### Exceptions Behind the Scenes

To understand the process of exception insertion by the tuner, consider a sample rule:

```yaml
- rule: Write below root
  desc: an attempt to write to any file
   directly below / or /root
  condition: root_dir and evt.dir = < and
   open_write
  exceptions:  - name: proc_writer
  fields: [proc.name, fd.filename]
```

And a stream of policy events with outputs such as:

```yaml
File below / or /root opened for writing (user=root user_loginuid=-1 command=/usr/local/bin/my-app-server parent=java file=/state.txt program=my-app-server container_id=a97d44bbe437 image=my-registry/app-server:latest
File below / or /root opened for writing (user=root user_loginuid=-1 command=/usr/local/bin/my-app-server parent=java file=/state.txt program=my-app-server container_id=a97d44bbe437 image=my-registry/app-server:latest
File below / or /root opened for writing (user=root user_loginuid=-1 command=/usr/local/bin/my-app-server parent=java file=/state.txt program=my-app-server container_id=a97d44bbe437 image=my-registry/app-server:latest
File below / or /root opened for writing (user=root user_loginuid=-1 command=/usr/local/bin/my-app-server parent=java file=/state.txt program=my-app-server container_id=a97d44bbe437 image=my-registry/app-server:latest
```

Then the tuner would add the following exception values to address the false positives:

```yaml
- rule: Write below root
  exceptions:
  - name: proc_writer
    values:
       - [my-app-server, /state.txt]
   append: true
```

For more background information on using exceptions, see the [Falco proposal](https://github.com/falcosecurity/falco/blob/master/proposals/20200828-structured-exception-handling.md) 

## Tuning Best Practices

### Auto Tuning

#### Learning/Onboarding to Sysdig Secure

The auto-tuner is best used when you are onboarding to Sysdig Secure with environments that have a lot of tooling. These might be flagged as nefarious actions, despite being benign. A database backup tool, for example, can trigger certain rules that are monitoring for data exfiltration.

{{% callout type="note" %}}

In many instances, exceptions are added out of the box and auto tuning is not needed.

{{% /callout %}}

#### Alternative Actions

In most cases, you may only get a handful of alerts from the same rule for the same workloads. See the [best practices](#tunable-exceptions) for tunable exceptions.

* Use Insights and review a grouped set of rules, using the [Tunable Exceptions](/en/docs/sysdig-secure/insights/#tunable-exceptions) to add exceptions for your workloads or applications
* Use the Events feed and review an event within a grouped policy, using the [Tunable Exceptions](/en/events-feed#tunable-exceptions) to add exceptions for your workloads or applications

**Review the auto-tuned exceptions after a learning/onboarding period**

After or during the auto-tuning period, you can review rules that have been modified in the [Rules Library](/en/manage-rules)  by viewing the rules that have "**Published By: Tuner 0.0.0**"
![](/image/auto-tuning-rules-library.png)

**How long should I leave auto-tuning on to learn about my environment?**

In many cases, a week should suffice, as most workloads would have likely taken any actions that were required

### Tunable Exceptions

The tunable exceptions feature is available in the [Secure Events feed](/en/events-feed#tunable-exceptions) and [Insights](/en/events-feed#tunable-exceptions). When you select tunable exceptions a modal will open.

**Should I apply all suggestions?**

No, in most cases the first suggestion will create an exception that will encompass all of the other suggestions. If the number of events shown on the top right of each suggestion are the same, it's likely that you are just duplicating exceptions. This is because the top suggestion targets a combination of the most events with the most targeted set of fields and values.

Take, for example, the following suggestions for the rule `Read sensitive file trusted after startup`.

![](/image/event-tuner.png)

The first suggestion successfully stops alerting for all fifteen events by specifying an exception, `known_sensitive_reader` with a specified process, `httpd`, when reading a specific type of password file, `/etc/shadow`.

The second suggestion targets the same 15 events, but specifies only the process `httpd`. If implemented, this would prevent `Read sensitive file trusted after startup` rule from triggering for any `httpd` process, including if the process opened other sensitive files such as `/etc/password`.
