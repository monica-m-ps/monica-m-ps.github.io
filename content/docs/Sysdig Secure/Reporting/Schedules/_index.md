---
linkTitle: "Schedules"
title: "Schedules"
weight: "9"
aliases:
  - /en/schedules/
description: "Sysdig Reporting is a highly scalable, powerful reporting platform. Use it to quickly create and schedule reports with large swathes of information. Reports are interactive, and can contain up to 30 days worth of data. You can export Reports in a variety of formats, secure data by filtering reports by Zone, and protect reports with a password."
---

The **Schedules** page lets you review, create, edit, enable and disable report schedules.

## Access Schedules

To access **Schedules**:

1. Log in to Sysdig Secure.

2. Select **Reporting** > **Schedules**.

  The **Schedules** page appears.

## Review Schedules

On the **Schedules** page, you can see a list of schedules associated with your account.

You can find specific reports with the search bar and the drop-down list.

For each report schedule listed, you can see:

- A toggle to enable or disable the schedule.
- **Name**: The name of the schedule.
- **Reports included**: Which report the schedule creates.
- **Frequency**: How often the report is generated.
- **Last Completed**: When this report schedule last created a report.

Click the three-dot icon beside any report schedule to **View History** for the report.

Select any schedule listing to edit the schedule. See [Configure a Schedule](#configure-a-schedule).


## Create a Schedule

To create a schedule:

1. Log in to Sysdig Secure.

2. Select **Reporting** > **Schedules**.

  The Schedules page appears.

3. Select **New Schedule** from the top right corner.

  The **New Schedule** page appears.

4. Fill out the configuration details. See [Configure a Schedule](#configure-a-schedule).

## Configure a Schedule

You can access the Schedule configuration page when you create a new schedule, or you edit an existing schedule. Here, you can configure the following:

- **Name**: The name of the schedule.
- **Description**: Enter any descriptive text for the schedule.
- **Enabled**: Toggle this to enable or disable the schedule.
- **Report**: From the drop-down, select the report you wish to schedule. To create a new report, see [Create a Custom Report](/en/docs/sysdig-secure/reporting/reports-manager/#create-a-custom-report).
- **Scope**: Select the scope of the report: **All Resources**, or **Specific Zones**. If you choose the latter, select the desired zones from the drop-down.
- **Timeframe**: Select the period of time you wish the report to gather data. The minimum is 1 day, and the maximum is 30 days.
- **Frequency**: Define how often you wish the schedule to run.
- **Notifications**: Optional: Select a notification channel if you wish to be notified when a report is created by a schedule, and ready to be downloaded. To create a notification channel, see [Set Up Notification Channels](/en/docs/administration/administration-settings/outbound-integrations/notifications-management/set-up-notification-channels/).
- **Format**: Select the format for you report. **PDF** is the default. **JSON** and **CSV** are only available when the report is a single table.
- **Password Protection**: Optional: Toggle this on to set a password on the report PDF that is created. Be sure to remember your password, as it cannot be reset once the report is created.

## Delete a Schedule

To delete a schedule:

1. Log in to Sysdig Secure.

2. Select **Reporting** > **Schedules**.

  The Schedules page appears.

3. Select the three-dot icon of the schedule you wish to delete.

4. Select **Delete**.