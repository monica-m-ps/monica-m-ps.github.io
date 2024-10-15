---
linkTitle: "Reports Manager"
title: "Reports Manager"
weight: "8"
aliases:
  - /en/report-manager/
description: "The **Reports Manager** is a powerful tool for report creation, management, and exportation. "
---

From the **Reports Manager**, you can:
- Download a report.
- Copy a report to a new custom report.
- Schedule the creation of new reports.
- View schedules associated with a report.
- View the history of previously generated reports.

## Access Reports Manager

To access the **Reports Manager**:

1. Log in to Sysdig Secure.

2. Select **Reporting** > **Reports Manager** from the left navigation bar.

    ![](/image/reporting.png)

   The **Reports Manager** page appears.


### Create a Custom Report

You can create a Custom Report in the following ways:

1. Log in to Sysdig Secure and select **Reporting** > **Reports Manager**.

2. Select the three-dot menu at the end of a row.

3. Select **Copy to Report**.

4. Choose a name and select **Create and Open**.

Alternatively, to create a Custom Report:


1. Log in to Sysdig Secure and select **Reporting** > **Reports Manager**.

    The **Reports Manager** page appears.

2. Select a Managed Report, such as `Registry Vulnerabilities`. You can identify **Managed** Reports by the **Report Type** column.

   The Report Detail page appears.

3. From the three-dot menu in the top right corner, select **Copy to Report**.

    ![](/image/report_copy.png)

4. Specify a name for your Custom Report, and select **Create and Open**.

    Your Custom Report is created.

### Set a Schedule

Report schedules let you determine when a pre-existing, defined report should be undertaken again.

To set a report schedule:

1. Log in to Sysdig Secure and select **Reporting** > **Reports Manager**.

2. Select a report.

  The Report Detail page appears.

3. Click the **Schedule** calendar icon.

   The **New Schedule** page opens.

4. Fill out the schedule details:

  - **Name**: Choose a descriptive name.
  - **Description**: Enter a description of the report. Optionally, specify the password if you choose to set a password for the report.
  - **Enabled**: If you wish to disable the schedule, toggle this option off.
  - **Report**: Choose which existing report you wish to schedule.
  - **Zones**: Choose which Zones the report should include. Be careful not to share a report of a Zone to a team that is not authorized to view that Zone.
  - **Timeframe**: Choose how long a span of time the report should cover, in days. The maximum is 9.
  - **Frequency**: Choose how frequently this report should be made. Use this in conjunction with **Timeframe** to cover large areas of time. For example, you could cover a whole month by setting the **Timeframe** to `7` and the frequency to `Monthly`.
  - **Notification**: Select where your completed reports should be sent. To set up a Notification Channel, see [Set Up Notification Channels](/en/set-up-notifications/).
  - **Format**: Select the format in which the report should be sent; PDF or CSV. Note the PDF format is limited to 7 columns and 250 rows. For larger reports, use CSV format.
  - **Password Protection**: Enable this option to secure the report with a password. Make sure to remember your password or save it in a password manager, as it cannot be recovered if forgotten.

## Navigate the Report Detail Page

Access The Report Detail page by selecting a report from the **Reports Mangers** page.

### Use the Time Bar

Use the Time Bar to navigate through time frames on the Report Detail page.

![](/image/reports_timebar.png)

Select between:
- **24 H** : Last 24 hours of data from the current time.
- **1 D** : Yesterday, the last full day of data in your current timezone.
- **5 D** : Last 5 full days of data ending at midnight yesterday in your current timezone.
- **7 D** : Last 7 full days of data ending at midnight yesterday in your current timezone.

Sysdig reevaluates reports multiple times per day, as well as when you update a workload's image.

### Filter by Zone

Use the Zones dropdown to include specific Zones in the report you have open. By default, all zones are selected.

Selecting a zone or zones will affect all the panels inside the report.  

The zone selection is not permanent and will not affect any scheduled reports. The zone is stored in the URL, so you can save the URL to go back to it later.

## Export Reports and Tables


In **Reporting**, you can download entire reports, or download individual tables from a report as in CSV or JSON format. You can also view when reports were previously downloaded.

### Download a Report

To download a report:

1. Log in to Sysdig Secure and select **Reporting** > **Reports Manager**.

2. Select a report.

   The Report Detail page appears.

3. Click **Export**.

4. Choose the **File Name** and **Format**.

   CSV and JSON formats are only available when the report only has one table or panel. Otherwise, reports are available to download in PDF format.

5. Click **Export** to download the report.

   Reports are downloaded as .zip files.

### Download a Table

To download a single table:

1. Log in to Sysdig Secure and select **Reporting** > **Reports Manager**.

2. Select a report.

   The Report Detail page will open.

3. Hover over the top right corner of the desired table to reveal the export icon.

4. Select the export icon.

   The **Export Data** window appears.

5. Select the desired **File Name** and **Format**.

   The available formats are CSV and JSON.

### View Downloads

To view previous downloads of Reports:

1. Log in to Sysdig Secure and select **Reporting** > **Reports Manager**.

2. Select a report.

   The Report Detail page will open.

3. Click the three-dot menu in the top right corner and select **View Downloads**.



## Explore Report Panels

On the Report Detail page, you can view panels of the reported data. Each panel contains predefined queries relevant to the data listed, which determine the columns displayed and the conditions available for that query. Editing the panel's filters will only affect the panel you are editing and will not impact any other panels within the report.