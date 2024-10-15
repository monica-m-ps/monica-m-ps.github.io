---
linkTitle: "Reporting"
title: "Reporting"
weight: "10"
aliases:
  - /en/reporting
description: "Sysdig Reporting is a highly scalable, powerful reporting platform. Use it to quickly create and schedule reports with large swathes of information. Reports are interactive, and can contain up to 30 days worth of data. You can export Reports in a variety of formats, secure data by filtering reports by Zone, and protect reports with a password."
---

{{% callout type="note" %}}


This module replaces the [**Vulnerabilities Management (VM) Reporting**](/en/vm-reporting.html) interface. The old report types now correspond to the following new report types:

- Image Pipeline = Pipeline Vulnerabilities
- Image Registry = Registry Vulnerabilities
- Runtime Workloads = Runtime Kubernetes Workload Vulnerabilities
- Runtime Hosts = Runtime Host Vulnerabilities
- Runtime Container = Runtime Container Vulnerabilities

{{% /callout %}}

The **Reporting** module comprises:

- **Reports Manager**: Use this to manage, create and delete reports.
- **Schedules**: Use this to manage and set report schedules.

## Report Types

Sysdig provides the following types of reports:

- **Managed** Reports are out of the box reports that may change over time, adding new panels and graphs as Sysdig adds new features and functionality. You cannot modify Managed Reports, but the you can change the date, time and zones within the scheduling configuration for a managed report. Managed Reports give you a bird's-eye view of available data. You can copy a Managed Report to create a Custom Report.

- **Custom** Reports are copies of managed reports that you can create and modify to your needs by applying filters at various levels in the report.