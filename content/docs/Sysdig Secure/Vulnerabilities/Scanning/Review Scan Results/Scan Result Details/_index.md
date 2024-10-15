---
linkTitle: "Scan Result Details"
title: "Scan Result Details"
weight: "10"
aliases:
  - /en/scan-result-details.html
---

{{% callout type="warning" %}}

**End of Life Notice**: The Sysdig Legacy Scanning Engine will reach its End of Life (EOL) on **December 31st, 2024**. After this date, it will no longer be supported or maintained. Please upgrade to our [New Scanning Engine](/en/docs/sysdig-secure/vulnerabilities) before December 31st, 2024 to ensure continuous service and support. For assistance, contact our support team or your account representative.
{{% /callout %}}

When you drill down into the Scan Results list, the details menu
provides a variety of ways to view vulnerability and policy violation
data at a glance.

-   Policy Summary views

-   Vulnerabilities summaries

-   Content summaries

These summaries provide:

-   An easy-to-parse view of why a specific image failed

-   Which rules generated the most Warn and Stop actions

-   Overview of how an image has performed against the various audit
    policies that have been put in place

-   Ability to filter for high-severity CVEs, and see which have an
    available fix

You can also download the Policy Summary to PDF and the Vulnerabilities
Summary to a CSV file.

## Policy Results Views

### Summary

The landing page of a Scan Results detail is the Policy Summary view.

You can:

-   Get a birds-eye view of scanning status

-   Drill down to a detail page

-   Click **Download as PDF** to get a full report, including all
    underlying CVEs.

-   **Added On:** See the date and time the scan was added.

-   **Added By:** See the mechanism by which the scan was reported.

    Possible values are: `Sysdig Secure UI`, `Node Image Analyzer`,
    `API`, `Sysdig Inline Scanner`, or `Scanning alert`.

-   **Re-evaluate:** Click the button to fetch the newest scan results

![](/image/scan_results_1.jpg)

### Select Dates for Past Scans

From the dropdown, select the date of the scan you'd like to analyze.

### Review Scanning Policy Details

Select a listed Policy to see details about the STOP and WARN actions
triggered in the **Evaluation**,

as well as the underlying **Rules** affected.

![](/image/result_policy.png)

## Review Vulnerability Summaries

Select either **Operating System**-related or **Non-Operating
System**-related **Vulnerability** summaries to review.

You can:

-   **Get a birds-eye-view** of vulnerability status

-   **Click a CVE number to get the full details and/or add it to an
    Exceptions list**

-   **Search or filter by severity:** Critical, High, Medium,
    Negligible, Unknown. **Also filter by whether it "Has a Fix".**

-   Click **Download CSV** to get the vulnerabilities data as a CSV file

-   **Open the Vulnerabilty Details panel** on the right by selecting an
    image from the list

-   **Added On:** See the date and time the scan was added.

![](/image/screen_shot_2021-03-17_at_12_29_23_pm.png)

### Red Hat Vulnerability Details

For Red Hat vulnerabilities, the details panel provides both the Sysdig
severity rating and the [equivalent severity label
](https://access.redhat.com/security/updates/classification) from the
Red Hat Security Tracker.

![](/image/redhat_vuln.png)

The labels are mapped as follows:

| Sysdig Severity | Red Hat Severity | Red Hat Definition                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|-----------------|------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Critical        | Critical         | This rating is given to flaws that could be easily exploited by a remote unauthenticated attacker and lead to system compromise (arbitrary code execution) without requiring user interaction. Flaws that require authentication, local or physical access to a system, or an unlikely configuration are not classified as Critical impact. These are the types of vulnerabilities that can be exploited by worms.                                                                   |
| High            | Important        | This rating is given to flaws that can easily compromise the confidentiality, integrity or availability of resources. These are the types of vulnerabilities that allow local or authenticated users to gain additional privileges, allow unauthenticated remote users to view resources that should otherwise be protected by authentication or other controls, allow authenticated remote users to execute arbitrary code, or allow remote users to cause a denial of service.     |
| Medium          | Moderate         | This rating is given to flaws that may be more difficult to exploit but could still lead to some compromise of the confidentiality, integrity or availability of resources under certain circumstances. These are the types of vulnerabilities that could have had a Critical or Important impact but are less easily exploited based on a technical evaluation of the flaw, and/or affect unlikely configurations.                                                                  |
| Low             | Low              | This rating is given to all other issues that may have a security impact. These are the types of vulnerabilities that are believed to require unlikely circumstances to be able to be exploited, or where a successful exploit would give minimal consequences. This includes flaws that are present in a program's source code but to which no current or theoretically possible, but unproven, exploitation vectors exist or were found during the technical analysis of the flaw. |
| Negligible      | n/a              | n/a                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

### Vulnerability Comparison

The vulnerability comparison allows users to compare two different tags
within the same repo to see which vulnerabilities are new or have been
fixed in version X compared to version Y.

This allows developers easily to compare the latest image to a previous
version to easily report on which vulnerabilities have been addressed
and which are new.

1.  Select an image from a line in the Scan Results list.

2.  From the `Compare To` drop-down, select another version of this
    image with which to compare.

3.  The comparison report is displayed.

    ![](/image/vuln_comp_results.png)

## Review Content Details

Navigate through node, ruby, python, java, OS packages, and the files in
a container to search for details about a particular package or file.

<img src="/image/374671248.png" width="900" height="448" />

