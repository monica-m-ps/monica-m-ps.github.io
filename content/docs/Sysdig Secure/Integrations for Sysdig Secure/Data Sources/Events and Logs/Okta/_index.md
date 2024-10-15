---
linkTitle: "Okta"
title: "Okta Integration"
weight: "10"
aliases:
  - /en/secure-okta
description: "Use Sysdig's Okta integration to pull logs from your enterprise's Okta installation into Sysdig Secure for threat detection and processing."
---

Okta, which manages internal and external access to systems with a single login, is the leading platform in Identity-as-a-Service. As such, it has been the target of [malicious attacks](https://blog.cloudflare.com/cloudflare-investigation-of-the-january-2022-okta-compromise/) which may expose users' most critical assets and services. Enterprises can protect this area of their infrastructure using Sysdig's log analysis Okta integration. 

## Prerequisites

*  Sysdig Secure SaaS and **Admin** permissions
* An Okta organization
* **Super Administrator** Okta account permissions

## Add Integration

The integration toggles between the Sysdig Secure UI and the Okta UI. 

### Start from Sysdig Secure

1. Log in to Sysdig Secure as **Admin** and select **Integrations > Events and Logs**.

   ![](/image/events_logs.png)

2. Select **Add Integration**.

   ![](/image/okta4.png)

3. Enter your **Okta organization name** or the **URL** for your [Okta organization](https://support.okta.com/help/s/okta-sign-in?language=en_US) and click **Launch Okta**.

4. You are transferred to the Okta admin dashboard, to install the Sysdig API Service Integration.

   ![](/image/okta_install_authorize.png)

5. By clicking on "Install & Authorize", you will be provided with a **Client Secret**. The authentication secret is periodically rotated for an additional layer of security.

   ![](/image/okta-secret.gif)

   Copy it and go back to Sysdig to paste it in the installation wizard.

   ![](/image/okta5.png)

6. Now return to Okta and do the same with the **Client ID**. 

   ![](/image/okta_app_detail.png)

7.  Click **Validate Connection** in the Sysdig wizard.

8. When connection is established, click **Complete** as prompted. 

   Logs from your Okta organization will be live-streamed to Sysdig. Sysdig performs threat detection through the [configured policies](#review-okta-policies-and-rules) and detections are reported as Runtime Events.

### Start from Okta

When you install the [API Service Integration app for Sysdig](https://help.okta.com/en-us/Content/Topics/apiservice/add-api-service-integration.htm) from Okta’s Admin Dashboard, the installation will prompt you to copy your Client Secret and Client ID. Save these to use on the Sysdig side.

![](/image/okta_service_integration_select.png)
![](/image/okta_app_detail.png)

Now: 

1. Log in to Sysdig Secure SaaS as **Admin** and go to **Integrations > Events and Logs**.

2.  Select **Add Integration** choose the option *If you've already installed the Sysdig API Integration*.

   ![](/image/okta6.png)

3. Enter the Client Secret, Okta Domain, and Client ID you saved from Okta (toggling back to the Okta interface if needed).

4. Click **Validate Connection**, and **Complete**. 

### Troubleshoot the Installation

If the installation is failing, you can: 

- Ensure that among the API Service integrations listed on Okta, the one you chose is called "Sysdig".
- Repeat the flow starting from Okta if you previously started from Sysdig, or vice-versa.
- Re-install the Sysdig API Service integration on Okta and copy all the values to Sysdig.
- Check that your Okta organization hasn't already reached the [maximum number of allowed Event Hooks](https://help.okta.com/oie/en-us/Content/Topics/automation-hooks/add-event-hooks.htm).

## Validate

### Check the Connection Status

Check the connection status of your integration in the Events & Logs page by navigating to **Integrations > Data Sources > Events & Logs.**

See [Events & Logs connection status](/en/events-logs#validate-account-connection).

### Review Okta Policies and Rules

You can review the suite of Okta-related managed policies delivered out-of-the-box and/or create custom policies of the type *Okta*. 

To see the standard managed policies: 

1. In Sysdig Secure, select **Policies** >  **Runtime Policies**.

2. For the **Managed Type** filter, select **Managed Policy** and for the **Select policy type** filter, select **Okta**.

   The list of default managed policies is displayed. Select one to review the rules that comprise it. 

   ![](/image/policy_okta1.png) 

   If desired, you can create custom Okta policies in the [usual way](/en/docs/sysdig-secure/policies/threat-detect-policies/manage-policies/#steps-to-create-a-custom-policy-from-scratch). 

### Check Event Feed for Okta Entries

1. In Sysdig Secure, select **Events > Event Feed**. 

2. Enter *Okta* in the free-text search. Select any resulting policies in the list to drill into event details. 

   ![](/image/okta_events2.png)


Events should arrive immediately after successful integration.  If nothing appears within five minutes, check the status of Sysdig SaaS on https://sysdig.com/company/sysdig-status/. 

## Delete an Integration

If you delete a configured integration: 

* The listiing is removed from the **Events and Logs** page
* Sysdig stops receiving logs from Okta
* Any created runtime events remain in the **Events** feed

## Appendix: Okta Setup
Sysdig relies on an [API Service Integration](https://help.okta.com/en-us/Content/Topics/apiservice/api-service-integrations.htm) to connect with your Okta organization. The integration provisions an [Event Hook](https://help.okta.com/oie/en-us/Content/Topics/automation-hooks/event-hooks-main.htm) configured to send Sysdig the following events categories:
- application.user_membership.add
- group.user_membership.add
- system.api_token.create
- system.org.rate_limit.violation
- user.account.lock
- user.account.privilege.grant
- user.account.reset_password
- user.account.update_password
- user.authentication.auth_via_mfa
- user.authentication.sso
- user.lifecycle.activate
- user.lifecycle.create
- user.lifecycle.deactivate
- user.lifecycle.suspend
- user.lifecycle.unsuspend
- user.mfa.factor.deactivate
- user.mfa.factor.reset_all
- user.session.start

