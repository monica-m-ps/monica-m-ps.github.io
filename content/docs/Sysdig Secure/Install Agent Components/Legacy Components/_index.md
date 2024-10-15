---
linkTitle: "Legacy Components"
title: "Legacy Components"
weight: "15"
no_list: true
aliases:
  - /en/kspm-components.html
  - /en/docs/installation/kspm-components/
  - /en/which-scanning/
  - /en/legacy-components/
---

## Background 

Sysdig Secure has multiple benchmark and compliance solutions (from oldest to newest): 

* Benchmarks (V1) - Retired Dec. 1, 2022
* Benchmarks (V2)/Compliance (legacy - Retired March 15, 2023
* [Compliance (Unified)](/en/docs/sysdig-secure/posture/compliance/legacy-versions/compliance-unified/) - Retired March 15, 2023, for SaaS. Still in use for On-Prem. 
* [Compliance](/en/docs/sysdig-secure/posture/compliance/) (Formerly called "Actionable Compliance")

KSPM components are used for the current Compliance module as well as other upcoming Sysdig Secure features.

Use this page to understand: 

- Why to upgrade your Sysdig Benchmark/Compliance version
- Which version you are currently using 
- Which upgrade path is appropriate and how to complete it

### Benefits of Upgrading

The new Compliance module moves beyond just finding violations to promoting remediations from source to run. 

Additionally:

* All resources are added to a central inventory data store along with their configuration information
* The policy evaluation happens in the backend using OPA (Open Policy Agent) as the policy engine
* 900+ controls are evaluated OOTB supporting:
  * Kubernetes (both vanilla and managed - EKS, GKE, AKS)
  * Linux
  * Docker
  * AWS
  * GCP
  * Azure
* Simple and intuitive creation of custom policies to match your organization’s needs 
* Unified experience across different target endpoints
* Clear and concise explanations of violations 

### Which Version Am I Using? 

If you are using **Unified Compliance**, the URL will have the form: 
[https://secure.sysdig.com/#/compliance/tasks](https://secure-staging.sysdig.com/#/benchmarks/tasks) 

![](/image/image3.png)

## Enable Compliance 

Enablement requires two basic steps: 

* Agent upgrade or agent install, using Helm enablement to take advantage of PR-integrated remediation (optional)

The precise upgrade/install steps differ depending which version you are currently using. When the basic steps are complete, the UI for actionable compliance will be populated with your environment's content. 

### Upgrading from any Other Version or New Install

Note that Sysdig is currently supporting two Helm chart versions: the [original](https://github.com/sysdiglabs/charts/tree/master/charts/sysdig)  and the [new](https://github.com/sysdiglabs/charts/tree/master/charts/sysdig-deploy),  and the parameters differ slightly between them. 

Use the new  chart if: 

* You are installing agents for the first time, or
* You installed using the new chart and now want to upgrade to enable Compliance. If necessary, check the [Secure endpoint for your region](/en/docs/administration/saas-regions-and-ip-ranges/). 

1. Replace the `sysdigcloud-benchmark-runner` with the KSPM collector. 

   * If you installed the Sysdig agent using the [original](https://github.com/sysdiglabs/charts/tree/master/charts/sysdig) chart, add the following flags:

     ```
     --set nodeAnalyzer.benchmarkRunner.deploy=false \
     --set kspm.deploy=true \
     --set kspmCollector.apiEnpoint=<endpoint> \
     ```

   * If you installed the Sysdig agent using the [new](https://github.com/sysdiglabs/charts/tree/master/charts/sysdig-deploy) chart, or are installing the agent for the first time, add the following flags:

     ```
     --set nodeAnalyzer.nodeAnalyzer.benchmarkRunner.deploy=false \
     --set global.kspm.deploy=true \
     --set kspmCollector.apiEndpoint=<endpoint> \
     ```

2. Disable existing compliance and benchmark tasks. In the UI, switch the `Enabled` toggle of each task. All tasks will be automatically disabled on March 15, 2023, and will no longer be able to be created or re-enabled.





