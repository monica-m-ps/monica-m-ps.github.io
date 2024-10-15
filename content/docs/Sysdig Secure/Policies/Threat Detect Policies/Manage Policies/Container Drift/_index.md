---
linkTitle: "Container Drift"
title: "Container Drift Policy"
weight: "30"
aliases:
  - /en/drift-control.html
  - /en/drift
description: "Drift is when an environment differs from the state checked into a version control system. This can occur in software that was introduced, updated, or upgraded into a live environment. Sysdig's Drift Control feature identifies newly created, downloaded, or modified binaries that were not part of a container image before it started running. To implement Drift Control, create a Container Drift policy."
---

{{% callout type="note" %}}

Drift Control applies to containers only and does not work on hosts.

{{% /callout %}}

## Overview

Drift Control helps you:

* Prevent attacks by blocking container drift in production: Drift Control automatically flags and denies deviations from the original container, blocking malicious executables before damage is done.
* Enforce immutability best practice: Drift Control ensures that container software is not modified during its lifetime, driving good practices, consistency from source to run, and preventing actions that could be part of an attack.
* Enable easy and effective security: Teams are often overwhelmed by cloud-native complexity and blind to container drift, especially at scale. Now, security teams and IT can enable Drift Control for the entire container environment and immediately start protecting it.

The **Container Drift** policy has the following unique attributes:

* It includes only one pre-configured rule, which cannot be edited, and no other rules can be added to Drift Control policies.
* It is a custom policy, not a managed policy.

{{% callout type="note" %}}

Event notifications are generally limited to a frequency of once every five minutes. For details, see [Message Throttling in Sysdig Secure](/en/docs/administration/administration-settings/outbound-integrations/notifications-management/troubleshoot-notifications-channels/#message-throttling-in-sysdig-secure).

{{% /callout %}}

## Prerequisites

* Agent version 12.16 and above for Drift policy
  
* Agent version 13.0 and above for captures and container **stop/pause/kill** actions
* On agent version v13.1 and above:
  * `ignore_container_action`: Ignores kill, stop, pause container operation
  * `ignore_action`: Ignores all the actions including kill, prevent malware, prevent drift and container actions.
  
* Kernel version 5.0 and above (see **Prevent** action)

* Agent version 13.1.0 and above to **Detect** Volume Binaries
  * In v13.1.1, it is enabled by default.
  * In v13.1.0, add the following configuration to the `dragent.yaml` file:

    ```
    drift_deny_execution_from_volumes: true
    ```

See [Configure Drift Control](/en/configuration-library-secure/#configure-drift-control) in Agent Configuration.

## Create a Container Drift Policy

To configure a Container Drift Policy in the Sysdig Secure UI:

1. Select **Policies** > **Threat Detection** > **Runtime Policies** to display the Runtime Policies page.

2. Click **+ Add Policy** in the top right of the page.

3. Select **Container Drift** policy type.

   ![](/image/driftcontrol.png)

Next, you can configure the policy.

## Configure a Container Drift Policy

**Name**: Enter a policy name.

**Description**: Provide a meaningful and searchable description.

**Enabled/Disabled**: Toggle to enable the policy so that it generates events.

**Severity**: Choose the appropriate severity level as you would like to see it in the Runtime Policies UI: **High, Medium, Low**, or **Info**.

Policy severity is subjective and is used to group policies within a Sysdig Secure instance.

* **Note:** There is no inheritance between the underlying rule priorities and the severity you assign to the policy.

**Scope**: Define the scope to which the policy will apply, based on the type-dependent options listed. Be aware that auto-tuning is not used with Drift policies. If you have too many false positives, use Scope to tune them.

**Link to Runbook**: (Optional) Enter the URL of a company procedure that should be followed for events resulting from this policy. For example, [https://www.mycompany.com/our-runbook-link](https://www.mycompany.com/our-runbook-link).

If you enter a value here, then a **View Runbook** option will be displayed in any corresponding Event.

### Detect

**Drifted Binaries**: A drifted binary is any binary that was not part of the original image of the container. It is typically downloaded or compiled into a running container.

Click the toggle to dynamically detect the execution of drifted binaries.

If a detected binary attempts to run, Sysdig will create an alert, or the binary is denied from running if *Prevent* is enabled (see below).

**Volume Binaries**: Toggle this on to treat all binaries from mounted volumes as drifted. This will generate events and prevent the binaries from being executed.

**Exceptions**: A user-defined list of binaries that the Drift policy will ignore even if the binary has drifted. This can be used to reduce noisy events from a particular binary. Provide a full path of binaries separated by commas.

**Prohibited Binaries**: A user-defined list of binaries whose execution is blocked even if it was built with the image. Provide a full path of binaries separated by commas.

* **Note:** The Prohibited Binaries option was formerly called `always deny.`

### Actions

Determine what should be done if a Policy is violated.

#### Prevent

Toggle the **Prevent** action to stop the detected new executables from running.

**Note:** Depending on the kernel version, the agent will either:

* Kill the process once it is detected as drifted (kernel version <5.0)

* Prevent drifted processes from running (kernel version 5.0+)

  The latter method is preferable because it stops the drifted process from running for any period. The agent determines which mode to use at agent startup. One of the above-mentioned types of prevention is always available with 12.15+ agents. For example, Bottlerocket prevents by using the **kill** action.

#### Containers

Select what should happen to affected containers if the policy rules are breached:

* **Nothing (alert only):** Do not change the container behavior; send a notification according to Notification Channel settings.
* **Kill:** Kills one or more running containers immediately.
* **Stop:** Allows a graceful shutdown (10-seconds) before killing the container.
* **Pause:** Suspends all processes in the specified containers.

For more information about stop vs kill command, see [Docker's documentation](https://docs.docker.com/engine/reference/commandline/kill/).

   {{% callout type="note" %}}

The agent can be configured to prevent `kill/pause/stop` actions, regardless of the policy.

See [Ignore Container Actions at the Agent Level](/en/configuration-library-secure/#ignore-container-actions-at-the-agent-level).

{{% /callout %}}

#### Capture

Toggle Capture ON if you want to create a capture in case of an event, and define the number of seconds before and after the event that should be in the snapshot.

See also: [Captures](/en/docs/sysdig-secure/investigate/captures/).

#### Notify

Select a notification channel from the drop-down list to send notifications of events to appropriate personnel.

See also: [Set Up Notification Channels](/en/set-up-notifications).

## Check Events

When the policy is enabled, you can check for any detected events:

1. Log in to Sysdig Secure and select **Events**.

2. Type `Drift` in the filter bar to find where the Drift policy was triggered and drill down to examine the event details.

   ![](/image/drift_events.png)

## Caveats

There are certain conditions that the Drift policy will not catch.

### Script Execution using Binaries from the Base Image

Drift control tracks the execution of binaries that were not part of the original image. It does not cover cases where a script is executed leveraging binaries from the original image. For example, if an attacker downloads a Python script and leverages an existing Python binary from the base image, drift detection will not detect it at runtime.

### Persistent Volumes

As the Drift policy uses overlays for detection, execution from persistent volumes is not considered "drifted." Therefore, if a malicious binary is downloaded to a persistent volume and executed, it will not be caught.

Suppose you are using persistent volumes exclusively for data. In that case, you can set the following option in the agent config file to treat the execution of all binaries from persistent volumes as drifted:

```
drift_deny_execution_from_volumes: true
```

Use with caution. When this option is used with the Prevent action, the execution of any binary from any mounted volumes will be stopped.

### Container Limits

* For kernel versions below v5.13, Drift Control can monitor up to 128 containers per node.

* For kernel versions v5.13 or above, you can modify the container limit as described in [Configure Container Limits](/en/configuration-library-secure/#configure-container-limits-1).
