---
linkTitle: "Captures"
title: "Captures"
weight: "11"
aliases:
  - /en/captures-122718.html
  - /en/capture
  - /en/docs/sysdig-secure/investigate/captures/
  - /en/docs/sysdig-secure/investigate/captures
description: "Sysdig capture files contain system calls and other operating system events. You can create captures manually, or configure certain Threat Detection policies, such as Workload and List Matching policies, to take captures in response to an event. Those captures can be then analyzed from Sysdig or opened with multiple open-source tools."
---

## Limitations

The Sysdig Agent can record only one capture per host at a time due to the volume of data collected. If multiple policies, each configured to create a capture, are triggered simultaneously on the same host, only the first event will store the capture successfully. Subsequent attempts to initiate captures will fail with the error: `Maximum number of outstanding captures (1) reached`. This issue also occurs with overlapping captures, often caused by lengthy capture durations.

## Access Captures

To access the **Captures**:

1. Log in to Sysdig Secure.

2. Select **Threats** > **Forensics** | **Captures**.

## Navigate Captures

The Captures page contains a table listing the:

- **Status**: When the status is **Ok**, the file has been successfully transmitted from the Sysdig agent to the storage bucket, and is available for download and analysis.
- **Name**: The capture file name.
- **Time**: The time the capture was taken.
- **Duration**: The period of time captured.
- **Trigger**: The cause that triggered the capture.
- **Infrastructure**: The host the capture was retrieved from.

You can also search for captures using the search bar, or filter them by **Error** or **Expiring**.

## Take a Capture

There are two ways to take a capture:
- Manually
- As a Policy action

### Take a Capture Manually

To take a capture manually:

1. Go to **Threats** > **Forensics** |**Captures** from the left navigation bar.

2. Select **Take Capture** from the top right corner.
    
    The **Take Capture** window appears.

   ![](/image/take_capture2.png)

3. Specify the following information:

   - **Name**: Define the Name of the capture, also used as capture file name.
   - **Host Name**: Specify the Host where you want to take a capture
   - **Container ID**: Configure the Container ID to set as aa predefined filter when you open the Capture using Sysdig Inspect.
   - **Storage**: Choose your storage options. The default is **Sysdig Secure Storage**, where captures are stored for 90 days. To configure alternative storage options, see [Configure Capture Storage](/en/capture-storage/).
   - **Duration**: Define the duration of the capture. The default time is 5 seconds; the maximum length is 300 seconds (five minutes).
   - **Filter**: Optionally, set a filter for the capture. This restricts the data streaming captured (syscalls, i/o streams), while it doesn't apply to the inspector tables (file descriptors, network connections, open ports, thread table, process list, containers). Applicable filters match the syntax for Falco conditions. See [Condition Syntax](https://falco.org/docs/reference/rules/supported-fields/).

4. Click **Start**.

   The capture is taken.

5. After the capture is complete, select whether to **Keep it** or **Discard** the capture. Capture you keep are displayed in the **Captures** report page.

### Take a Capture as Policy Action

To take a capture in Sysdig Secure, set up a policy with the capture Action available, or manually take a capture in the **Captures** page.

Policies that offer the capture action include:
- [List Matching](/en/list-matching-policy)
- [Workload](/en/workload-policy)

To configure a capture to be taken as Policy Action:

1. Select **Policies** > **Runtime Policies** from the left navigation bar.

2. Select an existing List Matching or Workload Policy, or create a new one.

3. Complete the configuration under the **Actions** section:

   - **Capture**: Toggle on or off.
   - **File Name**: Define the Name of the capture, also used as capture file name.
   - **Storage**: Choose your storage options. The default is **Sysdig Secure Storage**, where captures are stored for 90 days. To configure alternative storage options, see [Configure Capture Storage](/en/capture-storage/).
   - **Time interval**: Define the time interval of the capture. The default time is from 5 seconds before to 20 seconds after the event; the maximum length is 300 seconds (five minutes).
   - **Filter**: Optionally, set a filter for the capture. This restricts the data streaming captured (syscalls, i/o streams), while it doesn't apply to the inspector tables (file descriptors, network connections, open ports, thread table, process list, containers). Applicable filters match the syntax for Falco conditions. See [Condition Syntax](https://falco.org/docs/reference/rules/supported-fields/).

### Review a Capture

From the Captures page you can perform multiple operations on the previously taken Captures:
  - Open with Sysdig Inspect
  - Download
  - Delete

### Delete a Capture File

1. From the **Captures** page, select the capture files to be deleted.

2. Click the **Delete** (trash can) icon.

3. Click **Yes** to confirm deleting the capture, or the **No** to cancel.

### Review a Capture with Sysdig Inspect

To review the capture file with Sysdig Inspect:

1.  From the **Captures** page, navigate to the target capture file.

2.  Hover your cursor over the target capture file.

3.  Click on the **Sysdig Inspect** button to open Sysdig Inspect in a new browser tab.

{{% callout type="note" %}}

Sysdig Inspect is not available for captures over 200 MB.

{{% /callout %}}

### Download a Capture File

To download a capture file:

1. From the **Captures** page, select the three-dot menu on the side of a capture listing.

2. Select **Download File**.

 The capture downloads in .scap format. You can open this with:
  - [Sysdig Inspect](https://github.com/draios/sysdig-inspect)
  - [sysdig or csysdig](https://github.com/draios/sysdig)

### Delete a Capture File

You can delete a single capture file or multiple files.

To delete a single file:

1. From the **Captures** page, navigate to the target capture file.

2. Hover your cursor over the target capture file.

3. Select the three-dot menu icon from the right hand side of the capture listing.

4. Select the **Delete** (trash can) button.

3. Click **Delete** to confirm deleting the capture, or **No** to cancel.

To delete multiple files:

1. From the **Captures** page, select the capture files to be deleted.

2. Select the option **Delete** next to the number of selected captures in the top right corner.

3.  On the **Delete Captures** prompt, click the **Yes** button to confirm, or the **No** button to cancel.

## Disable Capture Functionality

Sometimes, security requirements dictate that capture functionality should not be available. To disable the Captures feature, see [Disable Captures](/en/disable-captures.html).

## Capture Files Storage

Sysdig capture files are stored in Sysdig's storage (for SaaS
environments), or in the Cassandra DB (for on-premises environments) by
default.
Captures saved in the Sysdig Storage (SaaS environments) for more than 90 days are automatically deleted.
Both environments have the option to use a S3-compatible custom storage, such as [Minio](https://min.io/) or [IBM Cloud Object Storage](https://www.ibm.com/cloud/object-storage). This lets you store captures for longer periods of time. To configure a custom storage, see [S3 Capture Storage](/en/docs/administration/administration-settings/outbound-integrations/s3-capture-storage/).
