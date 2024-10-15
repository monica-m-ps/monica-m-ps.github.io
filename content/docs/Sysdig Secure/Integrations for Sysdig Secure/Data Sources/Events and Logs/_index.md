---
linkTitle: "Events and Logs"
title: "Events and Logs"
weight: "60"
aliases:
  - /en/events-logs
No_list: true
description: "Events and Logs integrations allow Sysdig to ingest logs or events from third-party systems such as Okta and process them. This feature is in Technical Preview."  
---

From the Events and Logs page you can: 

* Add a new integration
* Review the name and status of an integration
* Search, filter, and sort by:
  * Keyword
  * Type (currently just Okta)
  * Status  (connected/disconnected)
* Delete an integration

![](/image/event_logs_del.png)

### Validate Account Connection

Use the **Status** column to validate the connection status of your log source:

1. Select **Integrations > Data Sources > Events & Logs**  and select a row from the list to open a detail panel.

2. Review the connection status and follow the prompts to resolve any errors or unknowns as needed.

The validation runs every 24 hours.

| Status Value  | Description                         |
|---------------|---------------------|
| Connected     | Your account is successfully connected.     |
| Error         | There is an issue with your integration. See [Troubleshooting an Okta Integration](/en/secure-okta#troubleshoot-the-installation) |
| Pending       | The account has been recently connected, and a validation will be run within an hour.            |
| Unknown       | Sysdig cannot determine the current status of the account.      |

{{% callout type="note" %}}

In the case of connection errors, if you remediate them the status will be updated on the page when the validation is run again, up to 24 hours later.

{{% /callout %}}

## Next Steps

* [Add Okta Integration](/en/secure-okta)