---
linkTitle: "Manage Scanning Alerts"
title: "Manage Scanning Alerts"
weight: "14"
aliases:
  - /en/manage-scanning-alerts.html
---

{{% callout type="warning" %}}

**End of Life Notice**: The Sysdig Legacy Scanning Engine will reach its End of Life (EOL) on **December 31st, 2024**. After this date, it will no longer be supported or maintained. Please upgrade to our [New Scanning Engine](/en/docs/sysdig-secure/vulnerabilities) before December 31st, 2024 to ensure continuous service and support. For assistance, contact our support team or your account representative.
{{% /callout %}}

Image scanning alerts, like all Sysdig alerts, can be configured to
notify users when an issue in the infrastructure arises. Scanning alerts
can be created for static images in the repository or for running
(runtime) images. Scanning alerts focus on when unscanned images are
added to the environment, images fail a policy evaluation, scanning
results change, or CVEs are updated.

Examples of when users might implement alerts:

- I want to know if there are new CVE updates for the 3 different
    images I handle

- I want to be notified if any of the common images from docker hub
    that are used all over my organization have a policy status that has
    changed

## Manage the Scanning Alert List

From the `Image Scanning` module, choose the `Alerts` tab. The Scanning
Alert list is displayed.

<img src="/image/alert_list.png" width="600" />

From here you can **search** for existing alerts, and **create**,
**duplicate** or **delete** alerts.

## Add an Alert

1. To create a new alert: From the `Image Scanning` module, choose the
    `Alerts` tab and click `Add Alert.`

2. Select either `Runtime` or `Repository` alert type.

3. Fill in the appropriate New Alert page (below).

### Create a Runtime Alert

Use Runtime alerts to scan running images and trigger a notification in
case of a policy violation, status change, or unscanned image added to
the environment. Enter the alert parameters and click `Save`.

<img src="/image/unscanned.png" width="600" />

#### Basic Parameters

Enter a `Name` and optional `Description`.

#### Scope

Use **`Everywhere`** or define a narrower scope.

#### Triggers

##### Unscanned Image

Check the box to trigger an alert. To have images scanned automatically
instead of simply triggering the alert, install the [Node Image
Analyzer](/en/docs/sysdig-secure/vulnerabilities/scanning/scan-running-images/#auto-scan-with-the-image-analyzer).

Note that this is a change from the way automated scanning was handled
in previous releases.

##### Scan Result Change

- `Pass/Fail:` Choose this option to be notified when an image that
    had previously passed now fails its policy evaluation.

- `Any Change:` Choose this option to be notified when there is any
    change on a previously scanned image result.

Note that if Scan Result Change is checked and a notification channel is
configured, an alert will be sent. If no channel is set up, nothing will
happen.

For example, the following image shows a Slack notification that was
triggered when "Any Change" was configured.

<img src="/image/374671147.png" width="600" height="230" />

##### CVE Update

Choose this option to be notified whenever a vulnerability is added,
updated, or removed from a running image.

#### Notification Channel

Click `Add Channel` to select a configured notification channel
(e.g. email) to be used for alert notifications.

If no notification channels have been defined for your Sysdig Secure
environment yet, see [Set Up Notification
Channels](/en/docs/administration/administration-settings/notifications-management/set-up-notification-channels/#set-up-notification-channels).

{{% callout type="note" %}}

OpsGenie and ViktorOps notification channels are not supported for image
scanning alerts.

{{% /callout %}}

### Create a Repository Alert

Use Repository alerts to scan static images in the repository and
trigger a notification in case of a policy violation, status change, or
a new image added to the environment. Enter the alert parameters and
click `Save`.

<img src="/image/alert_new_repo.png" width="700" />

#### Basic Parameters

Enter a `Name`and optional `Description.`

#### Registry/Repo/Tag

Enter the registry scope to be considered in the alert. Wildcards \* are
supported. If a wildcard is used for either the registry or the repo,
the only alert option will be `New Image Analyzed`.

#### Triggers

##### New Image Analyzed

Check the box to be alerted whenever a new image is analyzed, regardless
of the result.

##### Scan Result Change

- `Pass/Fail:` Choose this option to be notified when an image that
    had previously passed now fails its policy evaluation.

- `Any Change:` Choose this option to be notified when there is any
    change on a previously scanned image result.

Note that if Scan Result Change is checked and a notification channel is
configured, an alert will be sent. If no channel is set up, nothing will
happen.

##### CVE Update

Choose this option to be notified whenever a vulnerability is added,
updated, or removed from an image within the repository alert scope.

For example, the following image shows a Slack notification that was
triggered when "CVE Update" was configured.

<img src="/image/374671137.png" width="500" />

#### Notification Channel

Click `Add Channel` to select a configured notification channel
(e.g. email) to be used for alert notifications.

If no notification channels have been defined for your Sysdig Secure
environment yet, see [Set Up Notification
Channels](/en/docs/administration/administration-settings/notifications-management/set-up-notification-channels/#set-up-notification-channels).

## Edit an Alert

1. From the `Image Scanning` module, choose the `Alerts` tab.

2. Select the desired alert from the list.

3. Edit the alert trigger, scope, and notification channels as
    necessary, and click `Save`.

## Duplicate an Alert

1. From the `Image Scanning` module, choose the `Alerts` tab.

2. Select the desired alert from the list.

3. Click the `More` (three dots) icon and click `Duplicate Alert` from
    the drop-down, then `Yes` to confirm.

## Delete an Alert

1. From the `Image Scanning` module, choose the `Alerts` tab.

2. Select the desired alert from the list.

3. Click the `More` (three dots) icon and click `Delete Alert` from the
    drop-down, then `Yes` to confirm.
