---
linkTitle: "Pipeline"
title: "Pipeline"
weight: "11"
aliases:
  - /en/pipeline.html
  - /en/pipeline
  - /en/docs/sysdig-secure/vulnerabilities/pipeline
description: "The vulnerability **Pipeline** page shows the result from the scanning you have performed by using  the `sysdig-cli-scanner` tool. This Vulnerability Pipeline Scanner helps you manually scan a container image, either locally or from a remote registry. You can also integrate the `sysdig-cli-scanner` as part of your CI/CD pipeline to scan any container image after it is built but before it is pushed to the registry scanner."
---

Terms, such as Development,  CI/CD, Pipeline, or Shift-Left, refer to scanning performed on container images that are not yet executed in a runtime workload. You can scan these images using the `sysdig-cli-scanner` tool, and explore the results directly in the console or on the Sysdig Secure UI.

{{% callout type="note" %}}

This document applies only to the **Vulnerability Management engine,** released April 20, 2022. Make sure you are using the correct documentation: [Which Scanning Engine to Use](/en/docs/sysdig-secure/vulnerabilities/scanning/new-scanning-engine/)

{{% /callout %}}

## Prerequisite

Ensure that you have deployed `sysdig-cli-scanner`  in your environment. For information on installation, see [Install Sysdig CLI Scanner](/en/docs/sysdig-secure/install-agent-components/install-vulnerability-cli-scanner/).

## Review Pipeline Scan Results

You can explore the details for every image that has been scanned by executing the `sysdig-cli-scanner` in Sysdig Secure UI.

1. Log in to Sysdig Secure, and select to **Vulnerabilities** > **Pipeline** (under **Findings**).

   The **Policy Evaluation** column reflects the policy state at evaluation time for that image and the assigned policies.
   * **Failed:** If any of the policies used to evaluate the image is failing, the image is considered "Failed".
   * **Passed**: If no violation of any of the rules contained in any of the policies occurred, the image is considered "Passed".

2. Use the toggle beside the search bar to filter findings by **Passed** or **Failed**.

## Drill into Scan Result Details

Select a result from the Pipeline list to see the details, parsed in different ways depending on your needs.

### Overview Tab

Shows the recommendations, provides an overview of vulnerabilities and fixable packages associated with the selected image. You can also view the policy details on this tab. Click on the desired row to see detailed information in the **Packages**, **Policies**, or **Recommendations** tabs.

![](/image/pipeline_overview.png)

### Recommendations Tab

Shows image hierarchy, surfaces image layers, identify vulnerability issues in packages contained in each layer, and provides actionable instructions to fix the issues.

![](/image/pipeline-recommendations.png)

### Vulnerabilities Tab

In the **Vulnerabilities** tab, you can find expanded filters and a list of CVEs.

Click a CVE listing to open the full CVE details, including source data and fix information.

![](/image/pipe_vuln_tab.png)

The same security finding, such as a particular vulnerability, can be present in more than one rule violation table if it happens to violate several rules.

You can view the image hierarchy and the layers belong to the selected image. Click on a layer to view the packages, their versions, and  vulnerabilities detected in the layer.

You can refine your view further within the **Vulnerabilities** tabs:

![](/image/pipe_detail_filter.png)

* Search by keyword or CVE name
* Use filters: `Severity (>=)`; `CVSS Score (>=)`; `Vuln Type`; `Has Fix`; `Exploitable`.

### Packages Tab

This is organized by software packages, with expanded filters and clickable CVE cells. You can also view the image hierarchy and the layers belong to the selected image. Click on a layer to view the packages, their versions, and  vulnerabilities detected in the layer.

![](/image/pipe_content_tab.png)

You can refine your view further within the **Packages** tabs:

* Search by the keyword or the package name.
* Filter by **Package Type**.
* Filter by **Severity >=|=** **Critical**, **High**, **Medium**, **Low**, **Negligible**.

### Policies Tab

Shows the list of CVEs organized by failed policies. Use the toggle to show or hide passed policies. Click the CVE names for details.

![](/image/pipe_policies_tab.png)

### Detail Tab

Use the **Detail** tab to get the image information, such as the image ID, digest, author, labels used, and the operating system.

![](/image/pipeline_details.png)

## Accept Risk: Pipeline

You can choose to accept the risk of a detected vulnerability or asset. The process for handling **Accepted Risk** is the same for both Pipeline and Runtime.

Use the [Runtime instructions](/en/docs/sysdig-secure/vulnerabilities/findings/runtime/#runtime-risk-acceptance), with the following differences:

The pipeline scan results are point-in-time, so there is no automatic re-evaluation.

To trigger a new evaluation containing the accept:

* You must execute the pipeline process again over the same image.
* The N+1 scan will contain the accept.
