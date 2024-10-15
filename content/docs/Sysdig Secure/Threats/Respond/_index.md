---
linkTitle: "Respond"
title: "Respond"
weight: "3"
aliases:
  - /en/rapid-response.html
  - /en/rapid-response
  - /en/docs/sysdig-secure/investigate/rapid-response/
description: "Sysdig uses the Rapid Response feature and grants designated Advanced Users in Sysdig Secure the ability to remote connect into a host directly from the Event stream and execute desired commands there."
---

Finding the team or developer responsible for an application in a cloud or Kubernetes environment can take hours or days. Troubleshooting a live issue or security event may require faster investigation, to lower the mean time to repair (MTTR) of these events.

Rapid Response allows security teams to connect to a remote shell within your environment to start troubleshooting and investigating an event using the commands they are already accustomed to, with the flexibility they need to run the security tools at their disposal, directly from the event alert.

## Prerequisites

Contact Sysdig Support to have Rapid Response enabled for your SaaS Sysdig Secure environment.

To [Download Session Information](#download-session-information), the following prerequisites are required:

- OpenSSL v1.1.1 or above

- A properly configured [Custom S3 storage bucket](/en/install-rapid-response-k8s/).

## Process Overview

- [Install and configure the Rapid Response container](/en/docs/installation/rapid-response-installation/#rapid-response-installation) and ask Sysdig Support to enable Rapid Response.

- Create or configure teams of Sysdig Secure Advanced Users who should have Rapid Response privileges.

- Team members log in and manage workloads using Rapid Response shell.

- Review session logs to keep track of what has been done using Rapid Response.

## Configure Rapid Response Teams

{{% callout type="warning" %}}
Rapid Response team members have access to a full shell from within the Sysdig Secure UI. Responsibility for the security of this powerful feature rests with you - your enterprise and your designated employees.
{{% /callout %}}

For example, if you have an existing team called `CustomerResponse` with 40 members and you'd like five of those users to be granted Rapid Response capabilities. You could create a team called `CustomerResponse_RR` and add the five designated Advanced Users to it.

To create a team with Rapid Response permission:

1. Log in to Sysdig Secure as Admin.

2. Select **Settings** > **Teams**.

3. Select **Add team**.

4. Enter the team name and configure the team details.

5. Check the **Rapid Response** additional permission checkbox.

6. Select **Save**.

### Enable Rapid Response for an Existing Team

To enable Rapid Response for an existing team:

1. Go to **Settings** > **Teams**.

2. Choose the applicable team to display the Edit Teams page.

3. Select the **Rapid Response** checkbox in the additional permissions section and click **Save**.

## Start Rapid Response

1. Log in to the Sysdig Secure UI as a Rapid Response team member.

2. Select **Threats** > **Start Rapid Response**.

3. Select the host as prompted and click **Start Session**.

4. Enter the passphrase for that host.

5. Enter the two-factor authentication code that was emailed to you and click **Confirm**.

6. Begin your session.

   You can dock the terminal window at the bottom or right panel of your page, or as a separate screen.

## Launch a Rapid Response Session from Events Feed

1. Log in to the Sysdig Secure UI as a Rapid Response team member.

2. Select **Events** and choose an event from the list to open the detail pane.

3. Click **Respond: Launch Rapid Response**.

4. Enter the two-factor authentication code that was emailed to you and click **Confirm**.

5. Begin your session.

   You can dock the terminal window at the bottom or right panel of your page, or as a separate screen.

## Manage Rapid Response Logs

When reviewing the logs, you can download completed log sessions or close sessions that are live.

If you're using Sysdig SaaS, [configure a storage for Rapid Response logs](/en/docs/administration/administration-settings/storage-configure-options-for-capture-files/#storage-configure-options-for-capture-files) in order to store them.

The logs visible to you depend on the team and role under which you are logged in. Administrators will see the entire log list.

The Session Log list includes the session initiator, the timestamp, and the host name accessed.

### Download Session Information

If the session has been closed, you can download the content of the session from the UI (input and output) as an Open SLL-compatible gzip encrypted file.

To open the file, use the following command (`session-file` is the name of the downloaded file and `<passphrase>` is the passphrase you set up for that host during the installation):

```yaml
gzip -dc <session-file> | openssl enc -d -aes-256-ctr -pbkdf2 -k <passphrase>
```

### Close an Active Session

Rapid Response team members can review the Session Log list and close any active session by clicking **Close** from the **Actions** column.
