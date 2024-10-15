---
linkTitle: "Host Scanning"
title: "Host Scanning"
weight: "10"
aliases:
  - /en/host-scanning.html
  - /en/host-scan/
  - /en/host-scanner/
---

{{% callout type="note" %}}

This document applies only to the **Vulnerability Management engine,** released April 20, 2022. Make sure you are using the correct documentation: [Which Scanning Engine to Use](/en/docs/sysdig-secure/vulnerabilities/scanning/new-scanning-engine/). You also need to [enable it in Sysdig Labs](/en/docs/sysdig-secure/vulnerabilities/scanning/new-scanning-engine/#how-to-enabledisable-the-two-engines-ui).

{{% /callout %}}

 A "host" is any runtime entity where you could execute the Sysdig agent, including virtual machines, Kubernetes nodes, bare metal, and cloud-managed hosts such as EC2.

**Note:** Having the agent installed on the hosts is not required, but is recommended. The autocomplete feature on filters and searches depends on the Sysdig agent.

## Enable Host Scanning

### Option 1: Agent-Based

You can install the agent-based host scanner in several ways:

* On a [Kubernetes cluster](/en/install-kubernetes-secure/)
* On a [host as a container](/en/install-container-vuln-host-scan)
* As an [RPM package](/en/install-package-host-scanner)
* As a [binary application](/en/install-package-host-scanner)

See also:  the [Installation Requirements](/en/install-reqs-host-scan/).

Each of these methods enables the scanning of hosts.

#### Scanning Non-Kubernetes Containers

You can also extend the "host scanner" to scan for non-Kubernetes containers:

* On a host as a container
* As an RPM package
* As a binary application

These configurations are each described on the [Non-Kubernetes Container Scanning](/en/nonk8s-container-scan) page.

### Option 2: Agentless (Technical Preview)

You can deploy agentless vulnerability management host scanning  in AWS, GCP, or Azure using the Cloud Account onboarding wizard.

**Note**: You can apply tags to limit the scope on VPC/instance/disk access on your infrastructure. Do it before connecting the account, as described in the Prerequisites, or later on.

See [Connect Cloud Accounts](/en/cloud-accounts-secure/)  for AWS, GCP, or Azure, for installation details.

 See [Integrations > Data Sources | Cloud Hosts](/en/cloud-hosts) to check the status of the discovered resources.

### Current Feature Limitations

* No Risk Spotlight/In Use integration

### How Long until Host Scan Results Appear in the UI?

#### Agent-Based

* If the default parameter `nodeAnalyzer.nodeAnalyzer.hostScanner.scanOnStart=true` is set, then a scan will start just after the pod is ready. You can expect the results in a few minutes, ~15 minutes max.
* If this parameter is not set, results will be shown ~11 hours from installation.
* In all cases, scans are refreshed every 12 hours.
* Helm chart and Docker container installations behave the same.

#### Agentless

* It could take up to 15 minutes before scan results appear in Runtime.
* Scans are refreshed every 24 hours.

 See [Integrations > Data Sources | Cloud Hosts](/en/cloud-hosts) to check the status of the discovered resources.

## Usage

Once you have deployed the host scanner in your environment, the Runtime UI will integrate the findings alongside the runtime workload results, based on an out-of-the-box Vulnerability policy.

### Filter for Hosts

You can filter to find results of host scanning using the quick links in the banner at the top of the page, and/or the filter bar.

#### Agent-Based Result Filters

* Kubernetes cluster name
* Cloud account id
* Cloud account region
* Host Name
* Agent tags

#### Agentless Result Filters

* Cloud account id
* Cloud account region
* Host Name
* Cloud instance ID (text search)

See also, [Vulnerability Policies|Runtime](/en/docs/sysdig-secure/policies/vulnerability-policies/#runtime).

## Download Reports

You can schedule and download reports for scanning done on hosts as well as containers.

See [Vulnerabilities|Reporting](/en/docs/sysdig-secure/vulnerabilities/findings/reporting/).
