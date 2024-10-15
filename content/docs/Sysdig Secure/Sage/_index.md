---
linkTitle: "Sysdig Sage"
Title: "Sysdig Sage"
weight: "19"
aliases:
  - /en/sysdig-sage/
description: "Sysdig Sage is an AI cloud security assistant that helps you use Sysdig Secure CNAPP with multi-step conversations. You can ask questions in regular language about your runtime events from the Events Feed. Sysdig Sage summarizes Security events, provides insights into specific event, and offers context-aware recommendations based on the events it observes."
---

![](/image/sage-events.png)

## Sysdig Sage for Cloud Detection and Response

Sysdig Sage for Cloud Detection and Response (CDR) primarily targets users working in security operations responsible for incident response and forensics. It helps you reduce response time and speed up investigations. For example, Sysdig Sage for CDR can:

- Explain command lines
- Explain rules
- Interpret data
- Suggest areas to investigate for a particular issue
- Recommend next steps

Sysdig Sage's multi-level conversation:

- **Summarization**: Offers top statistics for runtime security events based on various groupings such as policy name, rule, event type, severity, and more.
- **Explainability**: Provides in-depth information about specific security events.

## Enable Sysdig Sage

To enable Sysdig Sage:

1. Log in to Sysdig Secure as an Admin.

2. Select **Settings** > **Sysdig Sage** | **Activation and T&C**.

If you are not an Admin, contact your Admin to enable Sysdig Sage for you. 

To enable Sysdig Sage for another user, you must create a custom role and team:

### Create a Custom Role for Sysdig Sage

To use Sysdig Sage, create a custom role and assign the required permissions:

1. Log in to Sysdig Secure as an Admin.

2. Select **Settings** > **Roles**.

3. Create a **New Role** for Sysdig Sage.

4. Grant the following permissions:


| Permissions                      | Settings                          |
| -------------------------------- | --------------------------------- |
| Sage: Full Access                | ![](/image/sage-permission.png)   |
| Events: Read only                | ![](/image/events-permission.png) |
| Captures and Investigate: Custom | ![](/image/audit-permission.png)  |


5. Click **Save**.

For more information about creating roles, see [Create a Custom Role](/en/docs/administration/administration-settings/access-and-secrets/user-and-team-administration/manage-custom-roles/#create-a-custom-role).

Next, you must create a team for Sysdig Sage Users.

### Create a Team for Sysdig Sage

We recommend you create a team for Sysdig Sage users for ease of operations:

1. Select **Settings** > **Teams**.

2. Select **Add team**. 

3. Configure the team and select **Save**.

For more information about team creation, see [Create a Team](/en/docs/administration/administration-settings/access-and-secrets/user-and-team-administration/manage-teams-and-roles/#create-a-team).

### Assign Users to the Sysdig Sage Role

Add users to the Team you set up for Sysdig Sage users and assign them the custom role you created for Sysdig Sage. See [Assign a User to a Team](/en/docs/administration/administration-settings/access-and-secrets/user-and-team-administration/manage-teams-and-roles/#assign-a-user-to-a-team) for more information.

1. Log in to Sysdig Secure as an Admin.

2. Select **Settings** > **Teams**.

3. From the **Teams** page, select the team you configured.

4. In the **Team Users** tab, select **Assign User**.

5. For **User**, select the user from the drop-down. For **Role**, select the Sysdig Sage role you configured.

6. Select **Save**.

The user now has access to Sysdig Sage.

## Get Started with Sysdig Sage

1. Log in to Sysdig Secure.

2. Click the Sysdig Sage icon on the top-right corner of the screen.

   ![](/image/sage-dashboard.png)

3. Enter your prompt.

   See [Example Prompts](#example-prompts) for more information.

4. Optionally, you can perform the following operations:

   | Operations               | Settings                      |
   | ------------------------ | ----------------------------- |
   | Clear Chat               | ![](/image/clear-sage.png)    |
   | Close the Sysdig Sage window    | ![](/image/close-sage.png)    |
   | Expand the Sysdig Sage window   | ![](/image/expand-sage.png)   |
   | Minimize the Sysdig Sage window | ![](/image/minimize-sage.png) |

## Example Prompts

| **PROMPT**                                             | **VALUE**                                                    |
| ------------------------------------------------------ | ------------------------------------------------------------ |
| What are the most critical events on my cloud infrastructure?<br>  What are my noisiest rules? <br> Which containers are generating the most events? <br>  What are my top 3 high severity events<br>  What are the most critical events on my cloud infrastructure?<br>How many events do I have for each severity? | Sysdig Sage can help you quickly summarize events on the Events Feed UI instead of analyzing them one by one. |
| What process generated the selected runtime event?<br>Help me understand the selected runtime event<br>Explain the rule that generated the selected runtime event   | Sysdig Sage is context aware. For example, Sysdig Sage is aware that the process that generated the selected runtime event. Sysdig Sage helps reduce analysis time down to a few seconds to get to the core interesting concepts. |
| Can you explain the command line option in this event? | Sysdig Sage gets you the right context with rich information and explanations. You no longer need  to leave Sysdig Secure and search on the Internet. |
| What cloud users are associated with runtime events? <br> What users are associated with critical events?  | Sysdig Sage gives you insight into the users who are associated with a selected event.|