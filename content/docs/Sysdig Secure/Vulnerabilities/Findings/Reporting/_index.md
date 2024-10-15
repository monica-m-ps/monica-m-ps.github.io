---
linkTitle: "VM Reporting"
title: "VM Reporting"
weight: "13"
aliases:
  - /en/vm-reporting.html
  - /en/reporting-secure/
description: "Use the Vulnerability Management (VM) Reporting interface in Sysdig Secure to schedule asynchronous reports about detected runtime or registry vulnerabilities, along with package and image data. You can schedule reports for runtime scanning, host scanning, and registry scanning."
---

{{% callout type="note" %}}

This document applies only to the **Vulnerability Management engine,** released April 20, 2022. Make sure you are using the correct documentation: [Which Scanning Engine to Use](/en/docs/sysdig-secure/vulnerabilities/scanning/new-scanning-engine/)

{{% /callout %}}

Reporting in Vulnerability Management allows you to:

* Create a report definition.
* Schedule its frequency.
* Define notification channels in which to receive the reports (email, Slack, or webhook).
* Preview how the data will appear (optional).
* Download the resulting reports in **CSV**, **JSON**, or **NDJSON**.
* Compression options: **ZIP** and **GZ**
* Optionally, generate a manual (unscheduled) report.

Regardless of the schedule, reports always include the data from the past 24 hours. Therefore, most users schedule a daily report to avoid having any gaps.

Past reports are stored for two weeks. Therefore, if you scheduled a weekly report, the list would only contain two records.

## Create a Report Definition

### Access Report Definition

1. Log in to Sysdig Secure with Advanced User or higher permissions.

2. Select **Vulnerabilities > Reporting**.

   The **Reporting** page appears.

   If you have previously created report definitions, you can click one to see the details.

3. Click  **Add report**.

   The **New Report** page is displayed.

   ![](/image/vm_reportnew.png)

4. Specify the name and description to identify the report.

5. Choose the entity you want and follow the appropriate instructions given below:

   * [Runtime Workloads](#runtime-workloads)
   * [Runtime Hosts](#runtime-hosts)
   * [Runtime Containers](#runtime-containers)
   * [Image Registry](#container-image-registries)
   * [Image Pipeline](#image-pipeline)

### Runtime Workloads

**Prerequisite**: These reports are available whenever you have the current Vulnerability Management module installed (not [Legacy Scanning](/en/docs/sysdig-secure/vulnerabilities/scanning/)).

Access the Report Definition and specify the following:

1. **Basic Information:** Define the basic information for the report:

   * **Name**: A unique name for the report
   * **Description**: A concise summary providing insight into the purpose of this report.
   * **Export file format**: CSV, JSON, or Newline Delimited JSON (ndsjon)

   Select Definitions:

   * **Entity**: Runtime Workloads

   * **Scope**: Entire infrastructure or subset from the drop-down menu

2. **Conditions:** (Optional) Click **Add condition** to choose conditions from the drop-down to filter reported items.  

   The available conditions include:

   * Image Name  (only for this Entity)
   * OS Name
   * [In Use](/en/docs/sysdig-secure/vulnerabilities/findings/runtime/#understanding-the-in-use-column)   (only for this Entity)
   * Package Name
   * Package Path
   * Package Type
   * Package Version
   * Vulnerability ID
   * [CVSS Score](https://nvd.nist.gov/vuln-metrics/cvss)
   * [CVSS Vector](https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator)
   * Vulnerability Publish Date
   * Exploitable
   * Fix Available
   * [Risk Accepted](/en/docs/sysdig-secure/vulnerabilities/findings/runtime/#accept-risk-runtime)
   * Severity
   * Vulnerability Fix Date

   **Example 1:** You want a report of all vulnerabilities with a **Severity >= High**, and for which a Fix is **Available**.
   ![](/image/report_conditions.png)

   **Example 2:** You want a report of all vulnerabilities that are **In Use** with **Accepted Risks**.

   ![](/image/report_conditions2.png)

3. **Schedule and Notifications**: Define the **Frequency** and time of day the report should be run.

The schedule determines when the report data collection begins. As soon as evaluation is complete, you will receive a notification in the configured notification channels.

Use the toggle to enable/disable **Notifications**. Slack, email or webhook notification channels you have configured will appear in the dropdown.

Since reports are typically large, the actual data is not sent to the notification channel; you receive a link to download it. You must be a valid Sysdig Secure user (Advanced User+) to access the link.

5. **Preview:** Click **Preview** to apply the configuration you've chosen and pull up on the center bar of the panel to see sample results.

6. Click **Save**.

### Runtime Hosts

**Prerequisite**: To get these reports, you must have [Vulnerability Host scanning](/en/install-secure/#vulnerability-host-scanning) installed.

Access the Report Definition and complete everything the same as for [Runtime Workloads](#for-runtime-workloads), with the following exceptions:

1. **Basic Information:** Define the report basic info:

   * **Name**: A unique name for the report
   * **Description**: A concise summary providing insight into the purpose of this report
   * **Export file format:** .csv, .json, or .ndjson
   * **Compression:** GZ or ZIP

   Select Definitions:

   * **Entity**: Runtime Hosts

   * **Scope**: Entire infrastructure or subset from the drop-down menu

2. **Conditions:** (Optional) Click **Add conditions** to choose from the dropdown how you want to filter the items reported on.  

   The available Conditions include:

   * Architecture * (only for this entity)

   * OS Name

   * Package Name

   * Package Path

   * Package Type

   * Package Version

   * Vulnerability ID

   * [CVSS Score](https://nvd.nist.gov/vuln-metrics/cvss)

   * [CVSS Vector](https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator)

   * Vulnerability Publish Date

   * Exploitable

   * Fix Available

   * [Risk Accepted](/en/docs/sysdig-secure/vulnerabilities/findings/runtime/#accept-risk-runtime)

   * Severity

   * Vulnerability Fix Date

### Runtime Containers

**Prerequisite**:  Ensure that you have the current Vulnerability Management module installed (not [Legacy Scanning](/en/docs/sysdig-secure/vulnerabilities/scanning/)) to get these reports.

Access the Report Definition and specify the following:

1. **Basic Information:** Enter the basic information:

   * **Name**: A unique name for the report
   * **Description**: A concise summary providing insight into the purpose of this report
   * **Export file format:** CSV, JSON, or Newline Delimited JSON (ndsjon)
   * **Compression:** GZ or ZIP

   Select Definitions:

   * **Entity**: Runtime Containers

   * **Scope**: Entire infrastructure or subset from the drop-down menu, which includes:
     * cloudProvider.account.id
     * cloudProvider.region
     * container.id
     * container.name
     * container.runtime.type
     * host.hostName

2. **Conditions:** (Optional) Click **Add conditions** to choose from the dropdown how you want to filter the items reported on.  

   The available Conditions include:

   * Image Name
   * OS Name
   * Package Name
   * Package Path
   * Package Type
   * Package Version
   * Vulnerability ID
   * [CVSS Score](https://nvd.nist.gov/vuln-metrics/cvss)
   * [CVSS Vector](https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator)
   * Vulnerability Publish Date
   * Exploitable
   * Fix Available
   * [Risk Accepted](/en/docs/sysdig-secure/vulnerabilities/findings/runtime/#accept-risk-runtime)
   * Severity
   * Vulnerability Fix Date

3. **Schedule and Notifications**: Define the **Frequency** and time of day the report should be run.

4. Use the toggle to enable/disable **Notifications**.

   Slack, email or webhook notification channels you have configured will appear in the dropdown.

   Since reports are typically large, the actual data is not sent to the notification channel; instead you receive a link to download it. To access the link, you must be a valid Sysdig Secure user with at least Advanced User permission.

5. **Preview:** Click **Preview** to apply the configuration you've chosen and pull up on the center bar of the panel to see the sample results.

6. Click **Save**.

### Container Image Registries

**NOTE:** To get these reports, you must have [Registry scanning](/en/install-registry-scan/) installed.

Access the Report Definition and complete everything the same as for [Runtime Workloads](#for-runtime-workloads), with the following exceptions:

1. **Basic Information:** Define the report basic info:

   * **Name**: A unique name for the report
   * **Description**: A concise summary providing insight into the purpose of this report.
   * **Export file format**: .csv, .json, or .ndjson
   * **Compression**: GZ or ZIP

   Select Definitions:

   * **Entity**: Image Registry

   * **Scope**: Entire infrastructure or subset from the drop-down menu

2. **Conditions:** (Optional) Click **Add conditions** to choose from the dropdown how you want to filter the items reported on.  

   The available Conditions include:

   * Image Name

   * OS Name

   * Package Name

   * Package Path

   * Package Type

   * Package Version

   * Vulnerability ID

   * [CVSS Score](https://nvd.nist.gov/vuln-metrics/cvss)

   * [CVSS Vector](https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator)

   * Vuln Publish Date

   * Exploitable

   * Fix Available

   * Severity

   * Vuln Fix Date

3. Container Images do not have runtime-specific scopes and metadata, like `Kubernetes cluster name`. Instead, they have registry-specific metadata and filters, such as `registry.vendor`. See the following table:

| Attribute                  | Possible Values                                              | Description                                                  |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `cloudProvider.name`       | `aws`, `azure`, `ibm`                                        | a shortname of the Cloud Provider, if applies                |
| `cloudProvider.region`     | One of the [AWS cloud provider regions](https://docs.aws.amazon.com/general/latest/gr/rande.html#regional-endpoints) | For AWS only, the region in where the cloud registry is located |
| `cloudProvider.account.id` | AWS account ID, for example, `012345678901`                      | For AWS only, the accountID in where the cloud registry is located |
| `registry.vendor`          | Installation configuration `config.registryType` value       | Specific value given in the installation to identify the vendor specific scanner type |
| `registry.name`            | Example: given `example.com/k8s-project/metrics-server:v0.6.1` it would be `example.com` | Registry hostname, up till first `/`                         |
| `registry.image.repo`      | Example: given `example.com/k8s-project/metrics-server:v0.6.1` it would be `/k8s-project/metrics-server` | Image repository including possible intermediate paths, from first `/` up till the `:` |

#### Scheduling

The **Frequency** with which reporting is configured in registry is closely linked to the refresh cycle, defined during [installation](/en/install-registry-scan/). The report frequency must be set up to be coherent with the `cronjob.schedule` cycle.
<br/>For example, the default value ( `"0 6 * * 6" as every Saturday morning`) is set to be performed weekly, which means daily reports would not make sense, since all results would be the same within those dates.

#### Limitations

* Registry Scanner does not handle [Vulnerability Policies](/en/docs/sysdig-secure/policies/vulnerability-policies/#overview).
* `cloudProvider.*` attributes will only be populated for AWS cloud provider.

### Image Pipeline

**NOTE:** To get these reports, you must have [Pipeline scanning](/en/install-secure/#vulnerability-pipeline-scanning) version 1.4.0 or above installed.

Access the Report Definition and complete everything the same as for [Runtime Workloads](#for-runtime-workloads), with the following exceptions:

1. **Basic Information:** Define the report basic info:

   * **Name**: A unique name for the report
   * **Description**: A concise summary providing insight into the purpose of this report
   * **Export file format**: .csv, .json, or .ndjson
   * **Compression**: GZ or ZIP

   Select Definitions:

   * **Entity**: Image Pipeline.

   * **Scope**: Entire infrastructure or subset from the drop-down menu.

2. 2. **Conditions:** (Optional) Click **Add conditions** to choose from the dropdown how you want to filter the items reported on.  

   The available Conditions include:

   * Image Name

   * OS Name

   * Package Name

   * Package Path

   * Package Type

   * Package Version

   * Vulnerability ID

   * [CVSS Score](https://nvd.nist.gov/vuln-metrics/cvss)

   * [CVSS Vector](https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator)

   * Vulnerability Publish Date

   * Exploitable

   * Fix Available

   * Risk Accepted

   * Severity

   * Vuln Fix Date

3. Container Images do not have runtime-specific scopes and metadata, like `Kubernetes cluster name`. Instead, they have registry-specific metadata and filters, such as `registry.vendor`. See the following table:

   | Attribute             | Possible Values                                              | Description                                                  |
   | --------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
   | `registry.name`       | ex.: given `example.com/k8s-project/metrics-server:v0.6.1` it would be `example.com` | Registry hostname, up till first `/`                         |
   | `registry.image.repo` | ex.: given `example.com/k8s-project/metrics-server:v0.6.1` it would be `/k8s-project/metrics-server` | Image repository including possible intermediate paths, from first `/` up till the `:` |

## Manage Reports

### View and Edit Report Definition

1. Select an entry in the **Reporting** list to see the detail panel.

   ![](/image/report_detail.png)

2. Click **Edit** to change the report definition parameters. You can also access this panel from the three-dot menu.

3. Make your edits, click **Preview** to see the Data Preview, and then **Save**.

### Download Reports

1. From the **Reporting** main page, download links for reports appear in the **Download** column.

   ![](/image/vm_listdl.png)

2. To see older reports, select  an entry in the Reports list to open the detail panel and select from the report download list.

   ![](/image/vm_detailID.png)

3. The report will be downloaded in the format you defined.

  The file is compressed in ZIP (.zip) or GZ (.gz) format.
  
4. Double-click the report to extract and view.

  You might require additional software to decompress the file.

### Generate Report Manually

1. Select an entry in the **Reporting** list to see the detail panel.
2. Click **Generate Now**.  

  A **Scheduled** entry will appear under **Latest reports**. In approximately 15 minutes, it will change to **Completed** and you can download the manually generated report.

### Duplicate a Report Definition

1. Choose the three-dot menu for a scheduled report.

2. Click **Duplicate**.

### Report Definition Retention

The scheduled and manually created reports are retained for 14 days.

### Delete a Report Definition

Be sure to download any needed reports before deleting the definition.

1. Choose the three-dot menu for a scheduled report.

2. Click **Delete**, click **Yes** when prompted.

   The report definition and all associated reports are deleted.
