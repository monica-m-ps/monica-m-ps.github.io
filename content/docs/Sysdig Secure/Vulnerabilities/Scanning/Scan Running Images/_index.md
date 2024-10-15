---
linkTitle: "Scan Running Images"
title: "Scan Running Images"
weight: "12"
aliases:
  - /en/scan-running-images.html
  - /en/image-scanning.html
---

{{% callout type="warning" %}}

**End of Life Notice**: The Sysdig Legacy Scanning Engine will reach its End of Life (EOL) on **December 31st, 2024**. After this date, it will no longer be supported or maintained. Please upgrade to our [New Scanning Engine](/en/docs/sysdig-secure/vulnerabilities) before December 31st, 2024 to ensure continuous service and support. For assistance, contact our support team or your account representative.
{{% /callout %}}

To automatically trigger scans of running images, install a node-based
image analyzer alongside the agent. Alternatively, you can scan
individual images manually from the UI.

## Auto-Scan with the Image Analyzer

### What is the Image Analyzer?

The (node) image analyzer (NIA) provides the capability to scan images
as soon as they start running on hosts where the analyzer is installed.
It is typically installed alongside the [Sysdig agent](/en/install-components-secure)container.

On container start-up, the analyzer scans all pre-existing running
images present in the node. Additionally, it will scan any new image
that enters a running state in the node. It will scan each image once,
then forward the results to the Sysdig Secure scanning backend. Image
metadata and the full scan report is then available in the Sysdig Secure
UI.

The analyzer performs the image analysis directly on the local host.
This poses several benefits:

-   **Automation:** Every image executed on your environments will be
    automatically scanned and checked against the vulnerability
    databases and configured scanning policies, without requiring any
    manual intervention

-   **Privacy:** Using local analysis, only image metadata is sent to
    the Sysdig backend, as opposed to pulling the entire image to be
    evaluated with backend scanning, which provides improved privacy

-   **Improved registry security:** Since the Sysdig backend will not
    pull the image from a registry, there is no need to configure
    registry credentials on the Sysdig-side, nor open up the registry
    endpoints to be accessed over public networks

If the node image analyzer is installed, there is no longer any need to
manually trigger running image scans.

### Installing the Image Analyzer

{{% callout type="note" %}}

If you have run the single line agent install with the
`--image-analyzer` flag, then this component is already running in your
infrastructure.

The feature is available for Kubernetes environments in Sysdig Secure
SaaS and in On-Premises version 3.5.1+.

{{% /callout %}}

Otherwise, the Image Analyzer is now deployed as a part of the [Node
Analyzer: Multi-Feature
Installation](/en/install-reqs-host-scan).

## Manually Scan an Image

If the node image analyzer is not installed, then when a new image is
added to a running environment it may need to be scanned manually. This
can be done from either the `Runtime` tab, or the `Scan Results` tab.

### From the Runtime Tab

To manually scan an image from the `Runtime` tab:

1.  From the `Scanning` module, choose the `Runtime` tab.

    ![](/image/newruntime3.png)

2.  Sort by `Unscanned` and select an image from the list of unscanned
    images.

    <img src="/image/scan_now.png" width="600" />

3.  Click `Scan Now` if the option is displayed. You may be prompted to
    install the Image Analyzer if the digest could not be detected.

### From the Image Results Tab

1.  From the `Scanning` module, choose the `Image Results` tab.

2.  Click `Scan Image`.

3.  Define the path to the image, and click `Scan`.

