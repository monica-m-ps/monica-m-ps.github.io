---
linkTitle: "Review Scan Results"
title: "Review Scan Results"
weight: "15"
aliases:
  - /en/review-scan-results.html
  - /en/docs/sysdig-secure/scanning/review-scan-results/
---

{{% callout type="warning" %}}

**End of Life Notice**: The Sysdig Legacy Scanning Engine will reach its End of Life (EOL) on **December 31st, 2024**. After this date, it will no longer be supported or maintained. Please upgrade to our [New Scanning Engine](/en/docs/sysdig-secure/vulnerabilities) before December 31st, 2024 to ensure continuous service and support. For assistance, contact our support team or your account representative.
{{% /callout %}}

When you have set up your build environment for scanning (if
applicable), added the desired registries, and either triggered a scan
manually or configured an alert to scan automatically, then an image
scanning report is generated.

There are different ways to access scan results:

- **Externally** (for developers): From an [external Continuous
    Integration (CI) tool](/en/docs/sysdig-secure/vulnerabilities/scanning/integrate-with-cicd-tools/#integrate-with-cicd-tools)
    such as Jenkins.

- **Internally** (for security personnel): From the **Runtime** tab or
    the **Scan Results** tab (formerly titled "Repositories") in the
    Image Scanning module of Sysdig Secure.

{{% callout type="note" %}}

**NOTE:** Images containing RPM packages with SHA512 hashes are not
supported.

{{% /callout %}}

## Scan Results Landing Page

Once a scan has been run, choose `Image Scanning > Scan Results` to see
the landing page.

![](/image/scan_results_landing.png)

From here you can:

- **Check quick-view charts for at-a-glance summaries of:**

  - Number of images scanned

  - Pass/fail status

  - Origins of image feeds

- **Search and filter results, by:**

  - Keyword

  - Pass/fail status

  - Origin (drop-down menu)

  - Registry (drop-down menu)

    **Save** or **Reset** a search from the three-dots menu to the right
    of the nav bar.

- **Sort the results list** by date.

- **Select an Image to see its Summary page.**

## Summary View

Select `Image Scanning > Scan Results` and select an Image to land on
the results summary.

![](/image/scan_results_summary.png)

On the Summary page you can:

- **Review results of vulnerability matching and policy evaluations in
    two separate sections**

- **Check the date and time** of the vulnerability match and the most
    recent policy evaluation. These usually differ.

- **Expand/collapse the policy breakdown** for ease of view and
    removal of visual clutter

- **Click Reevaluate Policies to trigger new policy results.**

- **Download results as a PDF**, including all the policy and
    vulnerability details.

Select detail pages from the left navigation to see detail views.

## Runtime View

`Runtime` provides an always-updated report on images that have been
running in your environment over the past 1 hour.

![](/image/newruntime3.png)

**In the left column:** view the `Entire Infrastructure` or drill down
to a namespace.

**In the Image Overview:** See the percentage of `Unscanned`, `Failed`,
and `Passed` images and click on each to get the relevant filtered list.

**Use the Search bar:** To find images based on `Registry`,
`Image Name,` or `Tag`.

You can drill down to the [Scan Result
Details](/en/docs/sysdig-secure/vulnerabilities/scanning/review-scan-results/scan-result-details/#scan-result-details).

### Unscanned Images

Select an unscanned image to manually trigger a scan.

### Scanned Images

Select a scanned image to drill down into the
[details](/en/docs/sysdig-secure/vulnerabilities/scanning/review-scan-results/scan-result-details/#scan-result-details): a `Summary` page,
`Policy` details, `Vulnerability` details, and `Content` violations
(e.g., licenses).
