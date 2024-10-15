---
linkTitle: "Overviews"
title: "Vulnerability Management Overviews"
weight: "1"
aliases:
  - /en/vulnerabilities.html
  - /en/vuln-overview
  - /en/docs/sysdig-secure/vulnerabilities/overview/
no_list: true
description: "Sysdig Secure offers a single page to identify, track, and initiate Vulnerability Management (VM) workflows. The Vulnerability Management Overview page covers all the scanning capabilities for images, workloads and hosts, as collected by the installed scanners: CLI, registry, host, and runtime."
---

The **Vulnerability Management Overview** page enables rapid identification of:

* Vulnerability Management (VM) trends
* Pervasive vulnerabilities and policy failures
* New risks
* Riskiest segments of your environment

The Overview provides reportable trend analysis that you can download as reports, export to create tickets, or share with team members. From the Overview, you can pivot into remediation workflows for specific CVEs, policies, architecture segments or coverage gaps.

{{% callout type="note" %}}

The **Vulnerability Management Overview** page is refreshed daily. Data might take up to 24 hours to appear.

{{% /callout %}}

## Prerequisites

* Sysdig Secure (SaaS) using current Vulnerability Management engine.

* A current Sysdig agent that includes the Vulnerability Management engine.

* The [Host Scanner](/en/host-scanner/).

* The [Runtime Scanner](/en/cluster-shield) for **Runtime**.

* [Sysdig CLI Scanner](/en/docs/sysdig-secure/vulnerabilities/findings/pipeline/#running-the-cli-scanner) for **Pipeline**.

* [Container Registry Scanner](/en/install-registry-scan/) for **Registry**.

* **Advanced User** permissions or higher.

## Getting Started with Vulnerability Management

1. For full vulnerability coverage, ensure you have completed the prerequisites.

2. Log in to Sysdig Secure and select **Vulnerabilities**.

   The out-of-the-box policies for Pipeline and Runtime vulnerabilities will work without further setup.

   <div style="max-width: 90%">
      <img src="/image/vuln_menu.png" />
    </div><br>

3. Choose [Pipeline](/en/pipeline), [Registry](/en/vuln-registry-scan/), or [Runtime](/en/runtime/) to see the scanning results.

4. Choose [Reporting](/en/docs/sysdig-secure/vulnerabilities/findings/reporting/) to configure schedules for creating downloadable reports on runtime vulnerability results.

5. To create or edit Pipeline or Runtime vulnerability Policies and Rule Bundles, select the relevant links from the [Policies](/en/docs/sysdig-secure/policies/vulnerability-policies/) tab in the navigation bar.

6. To accept the risk of detected vulnerabilities, configure an acceptance based on scope, justification, and length of time.

7. Optionally, download the vulnerability report in PDF format. To get a report in PDF format, select the entity and click **Download PDF**.

## Understand the UI

1. Log in to Sysdig Secure.

2. Under **Vulnerabilities | Overviews**, select one of the following:

    * **All Vulnerabilities**
    * **Pipeline**
    * **Registry**
    * **Runtime**

    Each page shows corresponding trend analysis that you can download as reports, export to create tickets, or pivot into remediation workflows for specific CVEs, policies, architecture segments or coverage gaps.

## Pipeline

The **Vulnerability Management Pipeline** page provides a quick overview of the pipeline scanning results. You can filter the results by **Zones** and **Pullstrings** to narrow down your search. Additional filters are also available to further refine the results by severity, vulnerability type, or policy.

![](/image/vm_pipeline_overview.png)

## Registry

The **Vulnerability Management Registry** page gives you a quick overview of the results from the registry scanning. You can filter by **Zones**, **[Vendors](/en/install-registry-scan/#supported-vendors)**, and **Registry** to narrow down the results. Additional filters are also available to further refine the results by severity, vulnerability type, or policy.

![](/image/vm_registry_overview.png)

## Runtime

The **Vulnerability Management Runtime** page gives you a quick overview of the results from runtime scanning. To narrow down the results, you can filter by the following:

* **Zones**: The Zones created in your environment
* **Resource Types** : Host, Workload, and Container
* **Cluster** : Kubernetes Cluster
* **Namespace**: Kubernetes Namespace
* **Hostname**: The available hosts in your Cloud or Kubernetes environment

![](/image/vm_runtime_overview.png)

Additional filters are also available to further refine the results by severity, vulnerability type, or policy.

## Filters

Select top-level filters to focus on a particular subset of vulnerability or policy data: severity and type.

#### Severity

Select any or all criticality level: **Critical**, **High**, **Medium**

#### Type

Select the package and CVE context variables: **Has Exploit**, **In Use**, and **Has Fix**

#### Date

The CVE trend chart displays data from the past 30 days. You can use the date selector or double-click on a day to see the Vulnerability panel results filtered for just that day.

Note that runtime and pipeline scans are performed twice a day, while registry scans are only performed after an action.

 All of the context filters apply to the widget on the page, the drilldown drawers, and exports of data.

## Scan Data Timelines

Each panel reports on the behavior of scans for the past 30 days. **Individual scan data** from:

* **Runtime** is retained for **14 days**
* **Registry** and **Pipeline** is retained for **90 days**
* **Package Details** in the the drill-down is available for **48 hours**

## Use Cases

### Vulnerability Management Usage

The top panel is designed to guide Vulnerability Management workflows.

This panel gives an overview of:

* **Trends of Unique Vulnerabilities** in the environment over the past 30 days
* **Most Pervasive** vulnerabilities
* **Recently Released** vulnerabilities, and
* **Namespaces** with the most vulnerability detections

These let you answer questions about their risk posture, such as:

* Are my CVE detection trending down?
* What are the most pervasive vulnerabilities?
* What are the most recent vulnerabilities (log4j-type event)?
* What is my most vulnerable application, segment or zone?

Each line item expands to a detail panel for further investigation.

The identified resources, vulnerabilities, or policies in the dropdown can be further **filtered** and **exported** via the Sysdig reporting or through a `.csv` file.

![](/image/vm_top_panel.png)

### Policy and Risk Management Usage

The Compliance Manager asks three fundamental questions.

* Which of my Compliance programs is struggling with control failures?
* Which of the controls is failing the most?
* Which of my applications, segments, or zones is failing the most policies?

The **Policy Panel** provides insight to all of these questions via the widgets:

* Top Failing Policies
* Top Failing Assets
* Top Failing Rule Sets

Dropdowns provide for more information and options to export.

## Sample Flows

### Identify Progress through Metrics

1. Choose a Filters for Phase (if applicable).
2. Choose CVE type filters.
3. Filter on segments of the infrastructure (if necessary).
4. Review the metrics graph to see trends.
5. Click on days to identify the difference between them.
6. Export any data to `.csv` from a subpanel.
7. Export the page including the graph to PDF for reports to executive.

### Identify a Problematic CVE

1. Filter by *Has Fix*, *Has Exploit*, and possibly *In Use*.
2. Filter by desired *severity*.
3. Review the *Top Recent* or *Top Pervasive* widget.
4. Identify a New or Particularly pervasive CVE.
5. Click into the dropdown.
6. View the assets and associated packages.
7. Choose:
   * Create a Report
   * Export the list of assets and packages to CSV
   * Click through to the results page of a single asset.

## Reports and Exports

There are various ways that the Vulnerability Management Dashboard can support your workflows through data exports.

### Download Scan Results

You can download scan results for pipeline, runtime, and registry scanning in PDF, CSV, and Zip format.

#### PDF

The downloaded file includes up to 100 rows and is sorted by severity. To download the desired scan results, use the **Download PDF** button found across the **Overview**, **Vulnerabilities**, **Content**, **Policies**, and **Detail** tabs.

#### CSV

You can download vulnerability report in CSV and Zip format for Pipeline, Registry, and Runtime scanning results.

1. Select the asset and click the **Vulnerabilities** tab.

2. Click **Export data**.

   ![](/image/export-csv-cve.png)

   The **Export Data** screen appears.

3. Select either CSV or Zip.

4. Click **Export**.

### Executive Reports

The dashboard can be scoped and filtered to support a focused view of trending and critical issues. Once filtered, you can export the Dashboard to PDF for inclusion in executive reports, audit artifacts, or briefings.

### Critical Vulnerability or Policy Tables

You can export data in tabular form from any of the widgets or panels on the VM Dashboard using the cloud download button on the panel. You can use this data in business intelligence or tracking tools.
