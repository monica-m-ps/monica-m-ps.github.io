---
linkTitle: "Risk Spotlight"
title: "Risk Spotlight (In Use)"
weight: "12"
aliases:
  - /en/risk-in-use
description: "The Risk Spotlight tool identifies vulnerable packages and libraries used in runtime workloads. Risk Spotlight powers the In Use field in the Vulnerability runtime scan results and Risk Spotlight integrations with 3rd-party software."
---

## Overview

Risk Spotlight determines which packages are effectively loaded during execution and are therefore a direct security threat to your infrastructure.

Prioritizing vulnerabilities that represent imminent risks to your organization is vital to a successful vulnerability management program. Images often contain hundreds of vulnerabilities. Multiplying this by the number of workloads running for any non-trivial infrastructure deployment, it is easy to see that the total number of potential vulnerabilities to fix is very large.

Many prioritization criteria are commonly used and accepted to start filtering the list: Severity and CVSS scoring, Exploitability metrics, Runtime scope, and other environmental considerations. Risk Spotlight is a new criterion, supported by observed runtime behavior, to add to the vulnerability management tool belt. Risk Spotlight can considerably reduce the working set of vulnerabilities that must be addressed as a priority.

### Terminology

* **EVE:** Effective Vulnerability Exposure, an earlier term. The installation settings may still refer to the *eveConnector* and *eveEnablement*.
* **Risk Spotlight:** Profiling insights applied to vulnerability prioritization and the **In Use** feature.

### Technology Details

To understand how Risk Spotlight is architected:

The Sysdig Agent components deployed for every instrumented node (host) continuously observe the behavior of runtime workloads. Some of the information collected includes:

* Image runtime behavior profile: accessed files, processes in execution, system calls.
* The Software Bill Of Material (SBOM) associated with container images used by runtime containers, including used packages and versions and the vulnerabilities matched by those.
* The combination of the SBOM and the Packages/Libraries identified as running make a Runtime Bill of Materials (RBOM).

By correlating these three pieces of information, we can differentiate between packages installed in the image and packages loaded at execution time. Sysdig propagates this information to vulnerability scan results, and can share it with partner integrations.

### Enable and Disable Risk Spotlight

Risk Spotlight requires the **Vulnerability Management** engine in Sysdig Secure SaaS. Make sure you are using the correct documentation: [Which Scanning Engine to Use](/en/docs/sysdig-secure/vulnerabilities/scanning/new-scanning-engine/).

Risk Spotlight works with all packages and formats that Vulnerability Management supports. See [Supported OSes, Packages and Languages](/en/docs/sysdig-secure/vulnerabilities/#supported-operating-systems).

## Understand the In Use Column

Risk Spotlight insights show up in the Vulnerabilities Runtime page in the In Use column.

The **In Use** designation lets you focus first on the packages containing vulnerabilities that are actually being executed at runtime. If an image has 180 packages and 160 have vulnerabilities, but only 45 are used at runtime, then much of the vulnerability notification noise can be reduced by focusing on the latter.

{{% callout type="note" %}}

Data will appear in the In Use column approximately 12 hours after the feature is deployed.

{{% /callout %}}

To use Risk Spotlight:

1. Log in to Sysdig Secure.

2. Select **Vulnerabilities** > **Findings** | **Runtime**.

   The Runtime Vulnerabilities page will appear.

3. Select the **In Use** button to filter the list by vulnerabilities being executed at runtime.

   ![](/image/riskspotlight.png)

   In each a particular listing, the number of **Critical**, **High**, and **Medium** severity **In Use** vulnerabilities is highlighted in the **In Use** column.

4. Select any one of the severities to drill down.

   ![](/image/riskspotlight2.png)